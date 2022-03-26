# File System

## File

이름을 통해 접근하는 단위

비 휘발성 보조기억장치에 저장된다.

리눅스 등의 운영체제에서 File은 데이터 저장 목적 뿐만 아니라 여러 장치를 논리적 단위로 관리하기 위한 목적으로 사용하기도 한다.

## metadata

file을 수정하는 것이 목적이 아니라 관리하기 위한 목적의 정보를 말한다.

파일 이름, 파일 접근권한, 형식, 시간, 유혈, 디스크상 저장위치, 크기 등

## File System

OS의 파일 관리 소프트웨어

저장메체에 있는 파일과 관련하여 저장 방식, 파일 관리, 파일 보호 등을 관리하는 소프트웨어이다.

파일을 빠르게 읽고 쓰고 삭제하는 등기본적인 기능을 원활하게 수행하도록 하는것이 목적이다.

파일 내용 뿐만 아니라 파일 metadata도 관리한다. (File System 구조)

- 커널 영역에서 동작 (사용자 영역X)

## File System 특징

크기가 일정한 블록들의 배열을 조직해서 파일이나 디렉터리를 만든다.

자료를 클러스터 또는 블록이라고 불리는 일정한 단위에 새겨 넣는데 이것이 파일 하나가 필요로 하는 디스크 최소 공간이다.

어느 부분이 파일이고 어느 부분이 공백인지를 구분하기 위해 배열에 표시를 해 둔다.

File System은 Disk에 저장된다.

## File System 구조

- 데이터 영역
    - 파일의 데이터 (파일 내용)
- 메타 영역
    - 데이터 영역에 기록된 파일의 이름, 위치, 크기, 시간정보, 삭제유무 등 파일의 정보
    - 윈도우 탐색기의 검색 기능은 이 메타 영역을 탐색하는 것이다.

## Directory and Logical Disk

Disk는 논리 Disk와 물리 Disk로 나뉜다.

OS가 보는 것은 논리 디스크이다. 파티션이라고 불린다.

- 하나의 물리 디스크를 여러 개 논리 디스크로 구성할 수도 있다.
- 여러 개의 물리 디스크를 하나의 논리 디스크로 구성할 수도 있다.

### Disk의 용도

- file System 용도
- swap area 용도 (메모리 swapping)

## open()

File open은 File의 metadata를 메모리에 올려 놓는 시스템콜이다.

### Memory & file

![memory_disk에서 file](https://user-images.githubusercontent.com/47437757/152656233-08ab14ca-0d21-4ff8-a880-9d3e230b55b4.png)

메모리엔 PCB가 있고

각 PCB 안에는 per-process file descriptor table과 system-wide 

open file table이 있고

Disk에는 각종 파일의 metadata와 content(내용)가 있다.

- per-process file descriptor table
    
    메모리에 올라온 파일의 metadata의 주소가 저장된 table
    
- system-wide open file table
    
    모든 process에서 open된 파일들을 관리하기 위한 table
    

### File open 과정

1. /a/b file을 open하는 시스템콜 발생
2. b에 접근하기 위해 metadata 찾기 시작
3. root directory (”/”)의 metadata를 통해 root에 content에 접근
4. root content 안에 a의 metadata 찾아 a content에 접근
5. a content 안에 b metadata를 찾아 메모리에 올림
6. b의 metadata 주소 값을 배열로 구성해 해당 배열의 index를 open함수의 반환 값으로 사용자 영역 프로세스에 전달
7. 배열 안의 b metadata 주소값 >  b metadata 안에 b의 위치 를 따라 b content에 접근
8. OS는 프로세스 요청에 따라 얻어낸 위치의 b의 용량만큼 메모리로 읽어들여 프로세스에게 전달

### buffer cashing

File Open 과정에서 읽은 메모리는 

사용자 프로세스에게 주기 전에 OS의 메모리 공간 일부에 copy되고 넘겨진다.

동일한 파일의 동일한 위치에 대한 read system call이 들어오면 copy해 둔 내용을 바로 전달한다.

이를 buffe cashing이라고 한다.

## File Protection

한 File은 여러 사용자가 같이 사용할 수 있기 때문에 각 파일에 대해 접근권한을 제어해야 한다.

3가지 방식이 있다.

### Access control matrix

행렬을 사용해 사용자들의 파일에 대한 권한을 모두 검사하는 방식

파일의 수가 사용자 수에 비해 압도적으로 많이 때문에 비효율적이다. (0이 많은 희소행렬)

해서 Linked List 자료구조를 사용해 효율적으로 구현할 수 있다.

파일마다 list를 만들어 사용자를 줄세우는 방식 & 사용자마다 list를 만들어 파일을 줄 세우는 방식 2가지로 나뉜다.

하지만 list 방식도 overhead가 크기 때문에 일반적인 OS는 Grouping로 접근권한을 제어한다.

### Grouping

`owner`, `group`, `public` 세 그룹으로 나누고 rwx-rwx-rwx 9bit를 통해 파일 접근권한을 관리하는 방식

### Password

파일마다 비밀번호를 두는 방식

읽기 쓰기 실행 별로 비밀번호를 따로 두어야해서 관리 면에서 사용하기 어렵다.

## File System의 Mounting

![FileSystem_Mounting](https://user-images.githubusercontent.com/47437757/152656232-bbffdd59-fbf8-4d2e-b545-66ac36b5238a.png)

물리 디스크는 여러개의 논리 디스크(파티션)으로 나눌 수 있다.

서로 다른 파티션의 File System을 같이 사용하기 위해서

root file system의 특정 디렉토리 이름에 해당 파일 시스템을 마운트해두면 같이 사용이 가능하다.

## File System의 파일 접근 방법

- 순차접근
    - 처음부터 돌린 다음에 중간에 접근할 수 있는 방식
- 직접접근
    - 특정 위치를 접근한 다음에 바로 다음 위치로 접근하는 방식
    - 데이터 관리를 어떻게 하느냐에 따라 직접접근 파일일지라도 순차접근으로 접근해야할 수 있다.

---

## Allocation of File Data in Disk

Disk에 파일 저장법은 3가지가 있다.

### Contiguous Allocation 연속 할당

![contiguous_allocation](https://user-images.githubusercontent.com/47437757/152656231-dfb13cf4-efd2-458e-b0ff-74c0c5f77be2.png)

File System은 동일한 크기의 저장단위(논리적 블록-섹터)를 두고, 그 섹터 단위로 File을 나누어 저장한다.

위의 그림처럼 count 파일은 0부터 2까지, mail 파일은 19부터 24까지 

연속된 블록 n개에 File이 저장된다.

- 단점
    - 비어 있는 공간을 활용할 수 없다.
    - 수정을 통해 파일크기가 커지는 경우도 있는데, 연속할당이라면 크기 증가에 제한이 생긴다.
- 장점
    - 속도가 빠르다.
    - 직접접근이 가능하다. 
    ex) mail File의 4번째 블록부터 보고자할 때 4번쨰 부터 보는 것이 가능

### Linked Allocation 연결 할당

![linked_allocation](https://user-images.githubusercontent.com/47437757/152656228-85e12916-c0c7-4a6d-aac6-74c9ab2bf4e5.png)

Linked List를 사용해 비어있는 블록에 파일의 데이터를 저장하는 방법

블록에 `content`와 다음 블록 위치 `pointer`를 같이 저장한다.

- 단점
    - 직접접근이 불가능 (파일의 임의의 블록으로 바로 접근이 불가)
    - 속도가 느리다
    - 블록끼리 연결이 끊겨 다음 블록으로 접근을 못 할 가능성이 있다.
    - pointer까지 저장해야해서 섹터 하나로 저장할 수 있는 content를 두 섹터에 저장하는 낭비가 발생할 수 있다.
- 장점
    - 외부조각이 발생하지 않는다. (비어있는 블록이 없다)

### Indexed Allocation 색인 할당

![indexed_allocation](https://user-images.githubusercontent.com/47437757/152656227-f3e43efd-725f-4bcd-b07e-7e8ef20ce6e7.png)

File 블록의 index를 저장한 `index block`을 따로 두고 metadata에 index 블록 저장위치를 저장하는 방법

`index block`엔 파일이 저장된 블록 순번대로 블록의 index를 기록한다.

직접접근을 가능하게 한다.

- 단점
    - index 블록이 생기면서 저장공간이 낭비된다. (특히 큰 파일의 경우 index블록 하나로는 부족할 수 있다.)
- 장점
    - 외부조각이 발생하지 않는다.
    - 직접접근이 가능하다.

### Unix 파일 시스템의 구조

![Unix_FileSystem_Structure](https://user-images.githubusercontent.com/47437757/152656226-3108ee15-61c1-46b2-a0e4-7880cef5195f.png)

Unix의 파일시스템 저장구조는 4부분으로 나뉜다.

- Boot block
    - 모든 파일시스템의 가장 앞에 위치하는 부분
    - 부팅에 필요한 기본 정보가 저장되어 있다.
- Super block
    - 파일 시스템에 관한 총체적인 정보를 담고 있다.
    ex ) Inode list와 Data block 간의 경계선, Data block에서 어디가 빈 블록이고 어디가 사용중인지 등
- Inode list
    
    ![inode_list](https://user-images.githubusercontent.com/47437757/152656224-16bb0eda-b678-411a-8bc8-59636fd82dfc.png)
    
    - 파일의 metadata는 그 파일을 가지고 있는 디렉토리에 저장되지만, 모두가 그런 것은 아니다.
    - Unix는 일부의 metadata만 디렉토리가 보관하고 대부분의 metadata는 Inode list라는 별도의 위치에 보관한다.
    - 단 파일의 이름은 디렉토리가 보관한다.
    - 파일 하나당 Inode가 하나씩 생성된다.
    - 파일의 위치정보는 Indexed Allocation 기법을 사용하여 저장 관리한다.
- Data block
    - 파일의 크기가 큰 경우 index block이 이중 삼중 구조로 되어있는 것을 사용한다. 이런 index block은 Data block에 저장한다.
