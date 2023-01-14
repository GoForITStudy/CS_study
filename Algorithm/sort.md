### 버블 정렬 (Bubble sort)

- 한번 순회할 때마다 마지막 하나가 정렬됨
- 항상 O(N^2)
- but 이미 정렬되어 있는 배열의 경우, 최적화를 하면 O(N) → 한번만 탐색하므로

```
0번째 원소와 1번째 원소를 비교 후 정렬
1번째 원소와 2번째 원소를 비교 후 정렬
…
n-1번째 원소와 n번째 원소를 비교 후 정렬
```

```python
# 어떤 상황에서도 O(N^2)
def bubblesort(arr):
    n = len(arr)
    for i in range(n-1):
        for j in range(n-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

    return arr

# 이미 정렬이 되어 있으면 한번 탐색 후 더 진행 안함. => 이미 정렬되어 있는 배열에 대해서 O(N)
def bubblesort_optimized(arr):
    n = len(arr)
    for i in range(n-1):
        flag = False
        for j in range(n-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                falg = True

        if not flag:
            break
    return arr

```

### 선택 정렬 (Selection sort)

- 한 번 순회를 돌면 전체 자료 중 가장 작은 값의 자료가 0번째 인덱스에 위치하게 되므로 그 다음 순회부터는 1번 인덱스부터 순회를 돌며 반복하면 된다.
- 항상 O(N^2)

```
- 0번 인덱스 ~ n번 인덱스 중 가장 작은 값을 찾아 0번째 인덱스와 swap 한다. 
- 1번 인덱스 ~ n번 인덱스 중 가장 작은 값을 찾아 1번째 인덱스와 swap 한다. 
…
- n-1번 인덱스 ~ n번 인덱스 중 가장 작은 값을 찾아 n번째 인덱스와 swap 한다. 
```

```python
def selection_sort(arr):
    n = len(arr)

    # Traverse through all array elements
    for i in range(n):
        # Find the minimum element in remaining unsorted array
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j

        # swap the found minimum element with the first element
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

    return arr
```

### 삽입 정렬(Insertion sort)

- 인간이 직접 정렬하는 순서와 제일 흡사하다고 할 수 있다.

```
1. 카드를 하나 고른다
2. 내가 가지고 있는 카드를 쭉 살펴본다.
3. 현재 카드가 들어가야할 순서에 카드를 껴넣는다. 
```

- 삽입 정렬은 이 순서와 비슷하게 작동한다.

```
1. 0번 인덱스는 건너뛴다. 
2. 0~1번 인덱스 중 1번 인덱스 값이 들어가야할 위치를 찾아서 넣는다. 
3. 0~2번 인덱스 중 2번 인덱스 값이 들어가야할 위치를 찾아서 넣는다. 
…
4. 0~n번 인덱스 중 n번 인덱스 값이 들어가야할 위치를 찾아서 넣는다. 
```

![image](https://user-images.githubusercontent.com/47748246/200976440-d6c4322d-760f-4c89-8343-d0ea19643167.png)

- 최선의 경우 전체 자료를 한번만 순회하면 되기 때문에 O(n)의 시간 복잡도를 가지지만, 최악의 경우 O(n^2)의 시간 복잡도를 가진다.

```python
def insertionSort(arr):
    n = len(arr)
    # Traverse through 1 to len(arr)
    for i in range(1, n):
        key = arr[i]

        # Move elements of arr[0..i-1], that are greater than key, to one position ahead of their current position
        j = i-1
        while j>=0 and key < arr[j]:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key

    return arr
```

### 병합 정렬 (Merge sort)

- 분할 정복법
- 시간 복잡도 : O(nlogn)
- 공간 복잡도 : O(n) → 원소의 개수만큼 리스트를 쪼개고 따로 저장하고 있어야 함

```python
def mergeSort(arr):
    if len(arr) > 1:
        # Finding the mid of the array
        mid = len(arr)//2

        # Dividing the array elements into 2 halves
        L = arr[:mid]
        R = arr[mid:]

        # sorting the halves
        mergeSort(L)
        mergeSort(R)

        # merge : copy data to temp array L[] and R[]
        i = j = k = 0
        while i < len(L) and j < len(R):
            if L[i] <= R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        # checking if any element was left
        while i < len(L):
            arr[k] = L[i]
            i += 1

        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1
```

### 퀵 정렬 (Quick sort)

- 병합 정렬과 마찬가지로 분할정복을 통한 정렬 방법
- 병합 정렬 : 분할단계에선 아무것도 안함. 병합하는 단계에서 정렬 수행
- 퀵 정렬 : 분할 단계에서 중요한 작업들을 수행, 병합 시에는 아무것도 안함

```
1. 입력된 자료 리스트에서 하나의 원소를 고른다. 이 원소를 피벗이라고 부른다
2. 피벗을 기준으로 리스트를 둘로 분할한다.
3. 피벗을 기준으로 피벗보다 작은 원소들은 모두 피벗의 왼쪽으로 옮긴다.
4. 피벗을 기준으로 피벗보다 큰 원소들은 모두 피벗의 오른쪽으로 옮긴다.
```

![image](https://user-images.githubusercontent.com/47748246/200976468-69b51f5e-79c8-48ae-8c5d-2d98bd8ab605.png)

```python
def partition(array, low, high):
    # Choose the rightmost element as pivot
    pivot = array[high]

    # Pointer for greater element
    i = low-1

    # Traverse through all elements
    # compare each element with pivot
    for j in range(low, high):
        if array[j] <= pivot:
            # If element smaller than pivot is found
            # swap it with the greater element pointed by i
            i = i+1

            # Swapping element at i with element at j
            array[i], array[j] = array[j], array[i]

        # Swap the pivot element with e greater element specified by i
        array[i+1], array[high] = array[high], array[i+1]

        return i+1

def quick_sort(array, low, high):
    if low < high:
        # Find pivot element such that
        # element smaller than pivot are on the left
        # element greater than pivot are on the right
        pivot = partition(array, low, high)

        # recursive call on the left of pivot
        quick_sort(array, low, pivot-1)

        # recursive call on the right of pivot
        quick_sort(array, pivot+1, high)

array = [10, 7, 8, 9, 1, 5]
quick_sort(array, 0, len(array) - 1)
print(array)
```

[https://www.geeksforgeeks.org/quick-sort/](https://www.geeksforgeeks.org/quick-sort/)
