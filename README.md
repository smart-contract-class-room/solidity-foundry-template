## 0. 사전 준비
### 0-1. 필수 설치 도구
- Git  
- Node.js (v18 이상 권장)  
- pnpm 또는 yarn (선택)  
- Rust (Foundry 설치용, macOS/Linux는 자동 설치 스크립트로 해결되는 경우가 많음)  
- VS Code 혹은 선호하는 에디터  

### 0-2. Foundry 설치
터미널에서 아래 명령을 순서대로 실행합니다.

```bash
# 1) Foundry 설치 스크립트 다운로드
curl -L https://foundry.paradigm.xyz | bash
# 2) 터미널 재시작 후, Foundry 툴체인 설치
foundryup
```

설치 확인:

```bash
forge --version
cast --version
anvil --version
```

버전이 잘 출력되면 설치 완료입니다.

---

## 1. Foundry 프로젝트 구조  
이 템플릿은 기본적으로 다음 구조를 가집니다.

```text
round#/ 
├── foundry.toml
├── lib/
├── script/
├── src/
└── test/
```
- round#/ : 과제별 디렉토리 (예: round1, round2, ...)  
- src/ : 스마트 컨트랙트(Solidity) 소스 코드
- test/ : Foundry 기반 테스트 코드


---

## 2. 의존성 설치
Foundry 프로젝트를 처음 가져왔을 때 한 번만 실행합니다.

```bash
forge install
forge build
```
  
- forge install : lib/에 필요한 라이브러리(OpenZeppelin 등) 설치  
- forge build : 컨트랙트 컴파일  


---

## 3. 로컬 테스트 실행 (TDD 연습)
각 라운드 과제를 진행할 때는 **테스트 주도 개발(TDD)**을 기본 흐름으로 사용합니다.

### 3-1. 테스트 전체 실행
```bash
forge test
```

### 3-2. 특정 테스트 파일/테스트만 실행
```bash
# 파일 기준
forge test --match-path test/TokenVault.t.sol

# 테스트 이름 기준
forge test --match-test test_Deposit_Succeeds
```

실패하는 테스트를 기준으로, src/의 컨트랙트를 구현/수정하면서 점진적으로 통과시키는 연습을 합니다.

---

## 4. 가스 리포트와 최적화
Trainee는 가스 사용량을 인지하고, 최적화 포인트를 찾는 연습을 해야 합니다.

### 4-1. 가스 리포트 출력
```bash
forge test --gas-report
```

각 함수/테스트별 가스 사용량이 표 형태로 출력됩니다.
​

같은 로직을 다른 방식으로 구현했을 때 가스 차이를 비교해 보세요.

### 4-2. 최적화 예시 관점
- storage vs memory / calldata 사용  
- uint256 연산 순서 (amount * 1 / 1000 vs amount / 1000 * 1)  
- external vs public 함수 선택  
- revert("msg") 대신 custom error 사용  

각 라운드의 ASSIGNMENT.md에 가스 최적화 포인트가 함께 제시됩니다.


---

## 5. Hardhat + TypeScript (선택)
일부 라운드에서는 Hardhat/TypeScript 기반 과제도 진행할 수 있습니다.

### 5-1. Hardhat 프로젝트 초기화
```bash
mkdir hardhat-round && cd hardhat-round
npm init -y
npm install --save-dev hardhat typescript ts-node @types/node
npx hardhat  # → "Create a TypeScript project" 선택[web:287][web:290]
```

### 5-2. 기본 스크립트
- 컨트랙트: contracts/  
- 테스트: test/ (TypeScript 기반)  
- 실행:
    ```
    bash
    npx hardhat compile
    npx hardhat test
    ```  

Foundry와 Hardhat을 병행할 경우, 테스트/배포 패러다임 차이에 주목해 비교해 보세요.
