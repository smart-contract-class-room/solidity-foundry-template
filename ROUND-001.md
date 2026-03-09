# Round 1: Simple Vault (EASY)

## [지난 과제 요약]
- N/A (첫 번째 과제입니다.)

## [목표]
- Solidity의 기본적인 상태 관리 및 이더(Ether) 입출금 로직 이해
- `msg.sender`, `msg.value`, `mapping` 자료구조의 실전 활용
- Foundry를 이용한 TDD(Test Driven Development) 워크플로우 익히기

## [요구사항]
1.  **SimpleVault.sol 작성**:
    -   사용자는 이더를 예치(`deposit`)할 수 있어야 합니다.
    -   사용자는 자신이 예치한 금액만큼만 출금(`withdraw`)할 수 있어야 합니다.
    -   전체 예치된 총 잔액(`totalBalance`)을 조회할 수 있어야 합니다.
    -   사용자별 잔액을 조회할 수 있는 `balances` 매핑이 필요합니다.
2.  **보안/검증**:
    -   출금 시 잔액이 부족하면 트랜잭션이 실패(`revert`)해야 합니다.
    -   `revert` 시 적절한 에러 메시지를 포함해야 합니다.
3.  **Foundry Test (TDD)**:
    -   `testDeposit`: 예치 시 잔액이 정확히 기록되는지 확인.
    -   `testWithdraw`: 출금 시 잔액이 차감되고 이더가 전송되는지 확인.
    -   `testFailWithdrawInsufficientBalance`: 잔액 부족 시 출금 실패 여부 확인.

## [설명]
DeFi 프로토콜의 가장 기본은 자산을 안전하게 보관하고 관리하는 'Vault(금고)'입니다. 이번 과제에서는 복잡한 토큰 로직 대신 원형적인 이더(Native ETH)를 관리하는 금고를 설계하며 Solidity의 기본 문법과 EVM의 동작 방식을 익힙니다. 특히, Foundry의 `vm.prank`와 `vm.deal`을 활용해 다양한 시나리오를 테스트하는 습관을 기르는 것이 핵심입니다.

## [기대 효과]
- Solidity의 가시성(Visibility) 및 상태 변수 설계 능력 향상
- 이더 전송 방식(`transfer`, `send`, `call`) 중 가장 안전한 방식에 대한 고민
- 테스트 코드가 개발 프로세스의 일부가 되는 경험

## [힌트]
- 이더를 전송할 때는 `(bool success, ) = msg.sender.call{value: amount}("");` 방식을 권장합니다. 그 이유는 무엇일까요?
- `mapping(address => uint256)`은 특정 주소의 잔액을 추적하기에 최적의 자료구조입니다.
- Foundry 테스트에서 사용자 주소를 시뮬레이션하려면 `address(1)`과 같은 임의의 주소를 사용해 보세요.
