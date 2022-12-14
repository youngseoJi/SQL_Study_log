## 트랜잭션이란?
트랜잭션이란 여러 개의 작업을 하나로 묶은 실행 유닛이라고 한다.

각 트랜잭션은 여러개의 작업 중 하나라도 실패하면, 모든 작업을 실패한 것으로 처리에, 미완료된 작업 없이 모든 작업을 성공해야 한다. 
그러므로, 트랜젝션은 성공 또는 실패 라는 두 개의 결과만 존재한다.

데이터베이스 트랜잭션은 ACID라는 특성을 가지고 있습니다.

## ACID
ACID는 데이터베이스 내에서 일어나는 하나의 트랜잭션(transaction)의 안전성을 보장하기 위해 필요한 성질이다.


### Atomicity(원자성)
원자성은 하나의 트랜잭션에 속해있는 모든 작업이 전부 성공하거나 전부 실패해서 결과를 예측할 수 있어야 합니다. 하나의 단위로 묶여있는 여러 작업이 부분적으로 실행된다면, 업데이트가 일어났지만 누가 업데이트했는지 모르거나, 업데이트 날짜가 누락되는 등 데이터가 오염될 수 있습니다. 예를 들어 계좌이체를 할 때에는 다음과 같은 두 단계가 있습니다.
1. A 계좌에서 출금합니다.
2. B 계좌에 입금합니다.

계좌이체를 하려는데 A 계좌에서는 출금이 이뤄지고, B 계좌에 입금되지 않았다고 가정하겠습니다. 어디서 문제가 발생했는지 파악할 수 없다면, A 계좌에서 출금된 돈은 세상에서 사라지는 돈이 됩니다. 만약 은행에서 이런 일이 발생한다면, 은행은 더는 제 기능을 할 수 없을 겁니다. A 계좌에서 출금하는 일에 성공했지만, B 계좌에 입금하는 작업에 실패한다면 계좌 A에서 출금하는 작업을 포함하여 모든 작업이 실패로 돌아가야 한다는 것이 Atomicity(원자성)입니다.

원자성을 지켰다면 1번과 2번, 두 작업이 모두 성공적으로 완료되어야 합니다. 그렇지 않으면(둘 중 하나의 작업이라도 실패한다면), 하나의 단위로 묶여있는 모든 작업이 실패하게 만들어 기존 데이터를 보호합니다.

SQL에서도 마찬가지입니다. 특정 쿼리를 실행했는데 부분적으로 실패하는 부분이 있다면, 전부 실패하도록 구현되어 있습니다. 때때로 충돌 요인에 대해서 선택지를 제공합니다.


### Consistency(일관성)
두 번째는 데이터베이스의 상태가 일관되어야 한다는 성질입니다. 
하나의 트랜잭션 이전과 이후, 데이터베이스의 상태는 이전과 같이 유효해야 합니다. 트랜잭션의 모든 작업이 실행, 성공했을 경우의 데이터베이스는 데이터베이스의 제약이나 규칙을 만족해야 한다는 뜻입니다.


### Isolation(격리성, 고립성)
Isolation(격리성) 은 모든 트랜잭션은 다른 트랜잭션으로부터 독립되어야 한다 는 뜻입니다.
실제로 동시에 여러 개의 트랜잭션들이 수행될 때, 각 트랜잭션은 고립(격리) 되어 있어 연속으로 실행된 것과 동일한 결과를 나타냅니다.

격리성을 지키는 각 트랜잭션은 철저히 독립적이기 때문에, 다른 트랜잭션의 작업 내용을 알 수 없습니다. 그리고 트랜잭션이 동시에 실행될 때와 연속으로 실행될 때의 데이터베이스 상태가 동일해야 합니다.


### Durability(지속성)
Durability(지속성)는 하나의 트랜잭션이 성공적으로 수행되었다면, 해당 트랜잭션에 대한 로그가 남아야 합니다. 만약 런타임 오류나 시스템 오류가 발생하더라도, 해당 기록은 영구적이어야 한다는 뜻입니다.

예를 들어 은행에서 계좌이체를 성공적으로 실행한 뒤에, 해당 은행 데이터베이스에 오류가 발생해 종료되더라도 계좌이체 내역은 기록으로 남아야 합니다.
