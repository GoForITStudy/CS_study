# LinkedList
연결된 데이터를 저장할때 사용합니다.  
연결리스트를 구성하는 노드의 구조는 다음과 같습니다.
```
- node
    - data
    - Link(next)
```

- 장점:
    - 삭제와 삽입 O(1)
    - 길이가 정해지지 않은 데이터를 다룰때 유용
- 단점:
    - 메모리 요구량이 더 크다.
    - 원하는 위치 검색할 때 처음부터 다 확인해야한다
- 종류:
    - 단순 연결 리스트
    - 원형 연결 리스트
    - 이중 연결 리스트

### 배열과 비교
||배열|연결리스트|
|:-:|:-:|:-:|
|삽입/삭제|O(n)|O(1)|
|저장공간|연속한 위치|임의의 위치|
|탐색|O(1)|O(n)|

## 구현
단순 연결 리스트  
![스크린샷 2022-03-04 오후 9.16.51.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8a9c0a2e-f397-4c8b-a074-4c3c31d1ed0d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.16.51.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T005241Z&X-Amz-Expires=86400&X-Amz-Signature=ff17dc2588f9b6ef688358503fd0560c65bcd6800a303f7dded65e2989963629&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.16.51.png%22&x-id=GetObject)

- 노드
    ```python
    class Node:
        def __init__(self, item):
            self.data = item
            self.next = None
    ```
- 연결 리스트
    
    ```python
    class LinkedList:
        def __init__(self):
            self.nodeCount = 0  # 노드의 개수
            self.head = None    # 처음 노드
            self.tail = None    # 끝 노드
    ```

- 탐색 - k번째 원소 찾기
    
    ```python
    def getAt(self, pos):
        if pos < 1 or pos > self.nodeCount: # 범위 벗어났다면
            return None
        i = 1
        curr = self.head # 맨 앞 노드부터
        while i < pos:
            curr = curr.next
            i += 1
        return curr
    ```
- 리스트 순회
    ```python
    def traverse(self):
        answer = []
        curr = self.head
        while curr is not None:
            answer.append(curr.data)
            curr = curr.next
        return answer
    ```
- 길이 알아내기
    ```python
    def __len__(self):
        return self.nodeCount
    ```
- 원소 삽입  
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5f35bbd9-c3ec-408d-8b93-00791efccece/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.52.51.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T021927Z&X-Amz-Expires=86400&X-Amz-Signature=13462d67496785ee576b5b6259dad195002b55343d470699b28e1776764381f3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.52.51.png%22&x-id=GetObject)
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0714cadd-b649-4379-8fb6-fa521c80e9b8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.53.19.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T022303Z&X-Amz-Expires=86400&X-Amz-Signature=48d4b7386aa42016fa331706d12acac6fa1d67eadab7eba00c9dde335994f176&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.53.19.png%22&x-id=GetObject)
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b1db8e4c-3d40-42f0-b7fb-402279448dc9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.53.33.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T022326Z&X-Amz-Expires=86400&X-Amz-Signature=755a832e51721d12fab226f4de16b6afff1b84634fae317f6333bfa29f3d8af4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.53.33.png%22&x-id=GetObject)
    ```python
    def insertAt(self, pos, newNode):
        if pos < 1 or pos > self.nodeCount + 1: # 범위 밖 인덱스라면
            return False
        
        if pos == 1: # 맨 앞에 삽입
            newNode.next = self.head
            self.head = newNode
        else:
            if pos == self.nodeCount + 1: # 맨 뒤에 삽입
                prev = self.tail
            else: 
                prev = self.getAt(pos - 1) # 중간에 삽입
            newNode.next = prev.next
            prev.next = newNode
        
        if pos == self.nodeCount + 1:
            self.tail = newNode
        
        self.nodeCount += 1
        return True
    ```

- 원소 삭제  
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4f5a58c4-46ef-4588-9c0d-e2142b7aadc2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.54.11.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T022500Z&X-Amz-Expires=86400&X-Amz-Signature=4838f4f4318eeb653735c055650009165653534654ec8b838ff2c4b49abe9fb3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.54.11.png%22&x-id=GetObject)
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4568adfb-62f0-4cb4-a9d9-ca764da37c83/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.54.28.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220305%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220305T022534Z&X-Amz-Expires=86400&X-Amz-Signature=4cf643db9b1a0ba5d0977f6192aa13a9a1233e0106ba5a48eb564ea6592e8616&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-03-04%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25209.54.28.png%22&x-id=GetObject)
    ```python
    def popAt(self, pos):
        if pos <= 0 or pos > self.nodeCount:
            raise IndexError

        if self.nodeCount == 1: # 노드가 하나밖에 없는 연결리스트
            curr = self.getAt(pos)
            self.head = None
            self.tail = None
            self.nodeCount -= 1
            return curr.data

        # 삭제하려는 노드가 맨 앞이면 -> prev 없음. Head 조정
        if pos == 1:
            curr = self.head
            self.head = curr.next
        # 리스트 맨 끝 노드 삭제할때 -> Tail 조정 필요
        elif pos == self.nodeCount:
            curr = self.tail
            prev = self.getAt(pos - 1)
            prev.next = None
            self.tail = prev
        else:
            prev = self.getAt(pos - 1)
            curr = prev.next
            prev.next = curr.next
        self.nodeCount -= 1
        return curr.data
    ```

- 두 리스트 합치기
    ```python
    def concat(self, L):
        self.tail.next = L.head
        if L.tail:
            self.tail = L.tail
        self.nodeCount += L.nodeCount
    ```

### 참고자료
### 

[🖥  연결리스트 연산 PPT](https://docs.google.com/presentation/d/1JRd56Gl5Z2z6wXwRUnuzugsTLbS_1IzRWE4i9vDg-MA/edit?usp=sharing)

[자료구조09 - 연결리스트(Linked List)2](https://blog.naver.com/PostView.naver?blogId=glgkwls1&logNo=222343882010&categoryNo=13&parentCategoryNo=0)

[https://wayhome25.github.io/cs/2017/04/17/cs-19/](https://wayhome25.github.io/cs/2017/04/17/cs-19/)

[https://opentutorials.org/module/1335/8821](https://opentutorials.org/module/1335/8821)