참고자료)

[[자료구조] 그림으로 알아보는 B+Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Plus-Tree#case-3-%EB%B6%84%ED%95%A0%EC%9D%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%82%BD%EC%9E%85%EA%B3%BC%EC%A0%95)

[B+ Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)

# 멀티웨이 탐색 트리

트리의 노드가 **m개 이하의 가지**를 가질 수 있는 탐색 트리를 **m원 탐색 트리**라고 한다.

m원 탐색 트리는 이진 탐색 트리를 확장한 것이며, 이진 탐색 트리는 2원(2-way) 탐색 트리에 속한다.

# B+ 트리
<img src ='./image/DB 인덱스.jpg' width="600">
<img src ='./image/B플러스 트리 기본 형태.jpg' width="600">

**인덱스 된 순차 파일** 을 구성하는데 사용되는 자료구조이다. 실제 **DB의 인덱싱은 B+트리** 로 이루어져있다고 한다.

인덱스 된 순차 파일은 데이터를 차례로 처리하는 **순차 처리**와 특정 데이터를 직접 **탐색** 해 처리하는 두 가지를 모두 효율적으로 할 수 있다.

B+ 트리는 B 트리와 같이 각 노드가 적어도 1/2이 채워져야 하는 점은 같다. 그러나 **잎 노드를 순차적으로 연결** 하는 포인터 집합이 있다는 점에서 다르다. 

B+ 트리는 단말 노드로만 구성된 **순차 세트(ordered set)** 와 비단말 노드들로 이루어진 **인덱스 세트(index set)** 로 구분지을 수 있다.

- 인덱스  세트에는 탐색키 값만이 존재하며, 이 탐색키 값을 이용해 단말 노드에 있는 키 값을 신속히 찾을 수 있다.

B+ 트리에서는 잎 노드에서 포인터를 이용해 다음 키 값을 갖는 잎 노드를 가리키는 연결 리스트 구조를 갖는다. 또한 잎 노드들이 모든 키 값을 포함하고 있다.

- 순차 처리를 할 때, 잎 노드들의 연결 리스트 구조를 활용할 수 있다.
- 모든 잎 노드는 같은 레벨에 존재한다. 즉, 루트로부터 같은 거리에 있다.

## M차 B+ 트리의 특징

- 노드는 최대 *M*개 부터 *M*/2개 까지의 자식을 가질 수 있다.
- 노드에는 최대 *M*−1개 부터 [*M*/2]−1개의 키가 포함될 수 있다.
- 노드의 키가 *x*개라면 자식의 수는 *x*+1개 다.
- 최소 차수는 자식 수의 하한 값을 의미하며, 최소 차수가 *t*라면 *M*=2*t*−1을 만족한다. (최소 차수 *t*가 2라면 3차 B 트리이며, key의 하한은 1개 다.)

# 검색

특정 탐색 키 값에 대한 검색 연산은 인덱스 세트를 거쳐 순차 세트에 도달해 탐색 키에 해당하는 레코드 포인터를 획득하는 것으로 종료된다.

# 삽입 및 삭제

레코드가 삽입 또는 삭제되면 새로운 탐색키 또는 삭제된 탐색키에 대한 인덱스 엔트리를 트리에 반영하여야 한다.

레코드에 대한 수정은 일련의 레코드의 삭제와 새로운 레코드의 삽입 연산의 연속으로 볼 수 있다.

삽입 연산 시에는 노드에서  유지해야 할 탐색키 값과 포인터 수 증가로 인해, 노드를 분할split해야 하는 경우가 발생할 수 있다.

삭제 연산 시에는 노드에서 유지해야 할 탐색키 값과 포인터 수 감소로 인해, 노드를 주변의 노드와 병합merge해야 하는 경우가 발생한다.

또한, 하나의 노드가 단순 분할되거나 병합되면, B+ 트리의 높이 균형이 깨질 수 있으므로 이를 유지하는 기법 또한 필요하다.

## 삽입

삽입할 레코드의 탐색키 값이 속해야 할 단말 노드를 찾고, 그 단말 노드에 탐색키 값과 레코드 포인터 쌍(엔트리)을 삽입하는 과정으로 구성된다. 

또한, 레코드 삽입 후에도 노드 내에서 탐색키들이 순서를 유지할 수  있도록 해야한다.

노드에 저장 공간이 없을 경우(overflow 발생)에는 분할이라는 새로운 노드를 생성하는 과정이 동반된다.

- 노드에서 ┌M/2┐개의 키는 기존 노드에 두고 나머지 키는 새로 만들어진 형제 노드로 이동한다.
- 새로 만들어진 노드의 가장 작은 탐색키 값을 부모 노드에 추가한다.
    - 만약 부모 노드에 새로운 키값과 포인터를 추가할 공간이 없다면, 부모 노드에서 또 다른 분할 작업을 수행해야 한다. 즉, 분할 연산이 연쇄적으로 전파될 수 있다.

### 분할이 없는 경우
<img src ='./image/삽입 2-1.jpg' width="600">

### 분할을 하는 경우
<img src ='./image/삽입 3-1.jpg' width="600">
<img src ='./image/삽입 3-2.jpg' width="600">


## 삭제

먼저 검색 연산을 이용해 삭제할 대상 노드를 찾는다. 

삭제할 레코드의 탐색키를 이용해 삭제될 탐색키와 포인터를 포함하는 단말 노드를 찾고 해당 탐색키와 포인터를 삭제한다.

중복이 허용되는 탐색키에 대해서는 같은 탐색키 값을 갖는 복수 개의 인덱스 엔트리가 존재하기 때문에 삭제될 레코드를 가리키는 엔트리를 재검색하여 모든 엔트리를 찾아 단말 노드에서 제거한다.

제거된 엔트리의 오른쪽에 있는 모든 엔트리들은 빈 공간이 발생하지 않도록 왼쪽으로 이동시킨다.

만약 엔트리를 삭제한 단말 노드가 노드 유지 조건인  ┌(M-1)/2┐개의 키 값을 갖지 못하게 된다면, 이 노드를 없애거나 주변 노드와 키를 재분배하는 추가적 작업을 수행한다.

- 재분배 : 이웃 노드가  ┌(M-1)/2┐개보다 많은 키를 갖고 있는 경우
- 병합 : 키를 재분배하면 이웃 노드도 ┌(M-1)/2┐보다 적은 키 값을 갖게 되는 경우

키의 재분배가 발생하면 부모 노드에도 이로 인한 키 값의 변화를 반영한다.

부모 노드의 키 Ki 값은 Pi+1이 가리키는 노드의 가장 작은 탐색키 값으로 수정된다.

### 삭제할 레코드의 탐색키가 index에 없는 경우
<img src ='./image/삭제 1.jpg' width="600">

### 삭제할 레코드의 탐색키가 index에 존재하는 경우

- inorder successor : 중위 후속자

<img src ='./image/삭제 2-1.jpg' width="600">
<img src ='./image/삭제 2-2.jpg' width="600">