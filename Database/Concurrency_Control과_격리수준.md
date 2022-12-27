## 1. Transaction이란?

- 논리적으로 연관된 연산(operations)들로 구성된 하나의 집합을 transaction이라 한다.
- 한 트랜잭션을 구성하는 연산에는 다음과 같은 것들이 있다.
    - Read (A) operation
        - 데이터베이스에서 A 값을 읽고, 메인 메모리의 버퍼에 해당 값을 올린다. (저장 - 캐시)
    - Write (A) operation
        - 버퍼 메모리에 새로운 값 A를 쓴다. (이후 daemon 프로세스에 의해 데이터베이스 파일에 저장된다. )

- 예를 들어, 하나의 트랜잭션이 다음과 같은 operations들을 포함한다고 해보자
    1. R(A);
    2. A = A-1000
    3. W(A);
    
    해당 트랜잭션이 일어나기 전 A의 값을 5000이라고 할때, 트랜잭션 이후 A 값의 변화는 다음과 같다. 
    
    - 첫번째 operation이 데이터베이스에서 A를 읽고, 버퍼 메모리에 저장한다.
    - 두번째 operation이 A 값을 4000으로 감소시킨다.
    - 세번째 operation이 버퍼에 변경된 A 값을 저장한다. (이후 이 값은 DBMS의 daemon 프로세스에 의해 데이터베이스 파일에 저장된다.) 즉, 결과적으로 A의 값은 4000이 된다.

- 하지만 만약 transaction이 수행되던 도중 하드웨어나 소프웨어 결함으로 인해 fail이 발생한다면? 만약 해당 트랜잭션이 은행 트랜잭션일 때 사용자 입장에서는 돈이 인출되었는데, 은행 서버에는 반영이 안됬다면 돈이 그냥 사라진 것. 이를 방지하기 위해 데이터베이스는 다음과 같은 중요한 operations을 제공한다.
    - **Commit** : 한 transaction 내의 모든 instructions들이 모두 성공적으로 실행되면, 변화된 data들은 모두 DB에 영구적으로 저장된다.
    - **Rollback** : 만약 한 transaction 내의 모든 instructions들이 수행 도중 하나라도 fail 했다면, 그 트랜잭션 내에 성공한 instructions들도 모두 되돌려진다. 즉, 해당 transaction이 수행되기 이전 상태로 되돌아간다.

## 2. ACID 원칙

- **Atomicitiy (원자성) :** 한 트랜잭션 내의 모든 instruction들이 성공해야 해당 트랜잭션은 DB에 적용된다. 만약 그 중 적어도 하나가 실패할 경우, 해당 트랜잭션은 아예 적용되지 않는다. (그 이전 instruction도 모두 rollback 된다. )

- **Consistency (일관성) :** 트랜잭션이 실행을 성공적으로 완료되면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것을 의미한다. 예: 데이터베이스에서 정한 무결성 제약 조건을 항상 만족해야한다.

- **Isolation (격리성)**
    - 동시에 실행되는 트랜잭션은 서로 격리된다.
    - 격리성은 **직렬성**이라는 용어로 공식화됨
        - 직렬성 : 각 트랜잭션이 전체 데이터베이스에서 실행되는 유일한 트랜잭션인 것처럼 동작할 수 있다는 것
        - 즉, 실제로는 여러 트랜잭션이 동시에 실행됐더라도 트랜잭션이 커밋됐을 때의 결과가, 트랜잭션이 **순차적**(하나씩 차례로)으로 실행됐을 때의 결과와 동일하도록 데이터베이스가 보장하는 것

- **Durable (지속성)**
    - 트랜잭션이 성공적으로 커밋됐다면 하드웨어 결함이 발생하거나 데이터베이스가 죽더라도 트랜잭션에서 기록한 모든 데이터는 손실되지 않는다는 보장

## 3. Transaction Isolation Levels in DBMS

### 3-1. 트랜잭션 격리 수준(Isolation Level)과 필요성

- 데이터베이스의 ACID 원칙(그 중에서도 Consistency)을 지키기 위해서
    - locking이라는 개념 등장 : 한 트랜잭션이 DB를 다루는 동안, 다른 트랜잭션이 관여하지 못하게 막는 것.
    - 하지만 무조건적인 locking으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식으로 구현하면 → DB의 성능은 떨어지게 된다.
    - 반대로 응답성을 높이기 위해 Locking 범위를 줄인다면 잘못된 값이 처리 될 여지가 있다.
    - 그래서 최대한 효율적인 Locking 방법이 필요하다.  ⇒ Isolation level은 이 locking의 범위를 결정한다.

- isolation level 정도에 따라 다음과 같은 현상이 발생한다.
    - **Dirty Read**
        - transaction이 아직 commit되지 않은 데이터도 읽는 것을 말한다.
        - 만약 transaction A가 commit하지 않은 채로 한 row을 업데이트했다고 할때, 다른 transaction B가 해당 값을 읽었다고 하자(=Dirty Read). 만약 트랜잭션 A가 rollback될 경우, B는 사실상 일어나지 않았다고 여겨지는 값을 읽은 셈이 된다.
    - **Non Repeatable Read**
        - 한 트랜잭션이 같은 row를 2번 읽었는데 다른 결과가 나올 경우, Non Repeatable Read가 발생했다고 한다.
        - 예를 들어, 트랜잭션 T1이 데이터를 읽었는데, 이후 T2가 해당 데이터를 update한 뒤 commit 했다면, T1이 이후 같은 데이터를 읽었을 때 다른 값을 갖게 된다.
    - **Phantom Read**
        - 2개의 같은 쿼리가 실행됐는데, 그 결과 rows가 다를 경우 발생한다.

### 3-2. 트랜잭션 격리 수준(Isolation Level)의 종류

- Isolation level 조정은
    - 동시성이 증가하는데 반해 데이터 무결성에 문제가 발생할 수 있고
    - 데이터의 무결성을 유지하는데 반해 동시성이 떨어질 수 있다.
    - 레벨이 높아질 수록 비용이 높아진다.

- 위와 같은 현상을 기반으로 SQL standard는 다음과 같은 4개의 isolation level을 정의한다.
    1. **Read Uncommitted** 
        - 가장 낮은 격리 수준
        - 각 트랜잭션에서의 변경 내용이 `COMMIT`이거나 `ROLLBACK` 여부에 상관 없이 다른 트랜잭션에서 값을 읽을 수 있다.
        - 정합성에 문제가 많은 격리 수준이기 때문에 사용하지 않는 것을 권장한다.
        - 아래의 그림과 같이 `Commit`이 되지 않는 상태지만 `update`된 값을 다른 트랜잭션에서 읽을 수 있다.
        
        ![image](https://user-images.githubusercontent.com/47748246/209590039-684c4f3a-9f68-4175-906e-d416ce72bb2d.png)
        
        - 문제 : Dirty Read 현상 발생
            - 트랜잭션이 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있게 되는 현상
    
    1. **Read Committed** 
        - commit이 된 데이터만 읽는다.
        - RDBMS에서 대부분 기본적으로 사용되고 있는 격리 수준이다.
        - Dirty Read와 같은 현상은 발생하지 않는다.
        - 실제 테이블 값을 가져오는 것이 아니라 Undo 영역에 백업된 레코드에서 값을 가져온다.
        
        ![image](https://user-images.githubusercontent.com/47748246/209590047-bf45adf1-18a9-4be4-ad46-47c60261e321.png)
        
        - 문제 : Non Repeatable Read 현상 발생
            - transaction1이 Commit한 이후 아직 끝나지 않은 transaction2가 다시 테이블 값을 읽으면 다른 값을 얻게 된다.
            
    2. **Repeatable Read**  
        - 가장 제한적인 isolation level
        - 한 트랜잭션 내에서 같은 쿼리를 두번 수행할 때, 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제함으로써 두 쿼리가 상이하게 나타나는 비 일관성 방지
        
        ![image](https://user-images.githubusercontent.com/47748246/209590054-2e5011c9-2ff0-4870-af12-80434490b041.png)
        
    
    - 단점 : Phantom Read
        - 한 트랜잭션 안에서 일정 범위의 레코드를 두번 이상 읽을 때, 첫번째 쿼리에서 없던 유령 레코드가 두번째 쿼리에서 나타나는 현상을 말한다 .
        - 이를 방지하기 위해서는 쓰기 잠금을 걸어야 한다.
        
        ![image](https://user-images.githubusercontent.com/47748246/209590075-976fc6d5-7954-418e-a092-22395cb36083.png)
        
    
    1. **Serializable**
        - 가장 단순한 격리 수준이지만 가장 엄격한 격리 수준
        - 성능 측면에서는 동시 처리 성능이 가장 낮다.
        - SERIALIZABLE에서는 `PHANTON READ`가 발생하지 않는다. 하지만 성능 이슈 때문에 데이터베이스에서 거의 사용되지 않는다.

### Reference

[https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/](https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/)

[https://www.geeksforgeeks.org/concurrency-control-in-dbms/](https://www.geeksforgeeks.org/concurrency-control-in-dbms/)

[https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)

[https://doooyeon.github.io/2018/09/29/transaction-isolation-level.html](https://doooyeon.github.io/2018/09/29/transaction-isolation-level.html)

책) 데이터 중심 어플리케이션 설계
