## 2020 2. 3 TIL



Review___

배열 - 배열은 여러개니까- 컴퓨터가 자동으로 하게끔 - 결국 반복문이 필요



완전검색 - 모든 경우의 수 다 만들어놓고 - 탐색

-> 선택해야하는 숫자가 늘어나면 시간복잡도가 급격히 커짐에 주의



버블 솔트는 이중 포문, 시간 복잡도 n^2

카운팅 솔트는 세는 작업 따로, 자리에 지정해주는 작업 따로이니, 시간복잡도 n

-> 정수로 표현할 수 있는 자료에만 카운팅 솔트가 가능







### 2차원 리스트

- 1차원 리스트를 묶어놓은 리스트
- 선언: 행의 갯와 열의 개수를 필요로 함
- 다차원 리스트에서, 차원에 따라 index를 선언







### 리스트 순회

- 행렬에 하나도 빠짐없이 접근
- n+m개 원소를 빠짐없이 조사하는 방법



```python
arr = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
print(len(arr)) #행의 길이
print(len(arr[0])) #열의 길이

#행 우선순위
for i in range(len(arr)):
    for j in range(len(arr[0])):
        print(arr[i][j], end=' ')
    print()

#열 우선순회
for j in range(len(arr[0])):
    for i in range(len(arr)):
        print(arr[i][j], end=' ')
    print()
```







#### 지그재그 순회



```python
#M1
n = len(arr)
m = len(arr[0])
for i in range(n):
    for j in range(m):
        print(arr[i][j + (m-1-2*j) * (i % 2)], end = ' ') #마지막 기준점에서부터 얼마만큼 뺄 건지를 계산, 짝수번째 행 구별용

#M2
n = len(arr)
m = len(arr[0])
for i in range(n):
    if i % 2 == 0:
        for j in range(m):
            print(arr[i][j], end=' ')
    else:
        for j in range(m-1, -1, -1):
            print(arr[i][j], end=' ')
```







#### 원하는 데이터의 위치 찾기



```python
# 3 4
# 0 1 0 0
# 0 0 0 0
# 0 0 1 0

n, m = map(int, input().split())
l = [list(map(int, input().split())) for _ in range(n)]
new_l = [[i,j] for i in range(n) for j in range(m) if l[i][j] == 1]
print(new_l)
```







#### 전치행렬



```python
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
for row in arr:
    print(row)

for i in range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]

for i in range(3):
    for j in range(3):
        print(arr[i][j], end = ' ')
    print()
```







#### 인접한 값을 탐색

- 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법
- 위치에서의 변화에 대한 수치를 배열로 넣을 것, 델타(변화량)



```python
arr = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]]
dx = [-1, 0, 1, 0] #위, 오른, 아래, 왼
dy = [0, 1, 0, -1]

for x in range(len(arr)):
    for y in range(len(arr[0])):
        for i in range(4): #4번 탐색
            next_x = x + dx[i]
            next_y = y + dy[i]
            if next_x >= 0 and next_x < len(arr) and next_y >= 0 and next_y < len(arr[0]): #배열 범위의 설정이 필요(out of range error 방지)
                print(arr[next_x][next_y], end = ' ')
        print()
```







### 부분집합

- {1, 2, 3}의 부분집합
  - 1, 2, 3 각각을 포함하거나 포함하지 않는 여부 (공집합도 부분집합)
- 2^3 (3은 원소의 수)이 총 부분집합의 수
- 완전탐색 - 부분집합 - 모든 경우의 수 탐색, 이 때 쓰이는 방법이니 잘 기억



```python
bit = [0, 0, 0, 0] #포함 1, 포함 0, 포함 여부를 나타냄. 가지 수 2
arr = [1, 2, 3, 4]

# [0, 0, 0, 1] #마지막 원소만 포함
# [1, 0, 0, 0] #첫 번째 원소만 포함
# [0, 0, 0, 0] #아무것도 포함x
# [0, 1, 1, 0] #두 번째, 세 번째 요소 포함

for i in range(2): #선택지 둘
    bit[0] = i
    for j in range(2):
        bit[1] = j
        for k in range(2):
            bit[2] = k
            for m in range(2):
                bit[3] = m
                print(bit)
                for n in range(len(bit)):
                    if bit[n] ==1:
                        print(arr[n], end=' ')
                    print()
```







- 사방 탐색, 차의 절대값의 합 문제



```python
arr = [list(map(int, input().split())) for _ in range(5)]
dr = [-1, 0, 1, 0]
dc = [0, 1, 0, -1]

for i in range(len(arr)):
    for j in range(len(arr[i])):
        sum = 0 #원소가 새롭게 지정될 때 sum 초기화
        for k in range(4):
            nr = i + dr[k]
            nc = j + dc[k]
            if nr < 0 or nr >= len(arr) or nc < 0 or nc >= len(arr[i]): #원하는 조건이 아니면 꺼져
                continue #한 스텝만 건너 뜀
            # print(abs(arr[i][j] - arr[nr][nc]))
            sum += abs(arr[i][j] - arr[nr][nc])
        print(sum, end=' ')
    print()
```



행에 대한 접근, 열에 대한 접근

(열의 개수가 일정하지 않을 때 len(arr[0])은 어폐)

```python
for i in range(len(arr)):
    for j in range(len(arr[i])):
```







### 부분집합 -bit

- 비트 연산자

- 비트 연산이란, 2진수로 바꾼 뒤 연산(shift, and)하는 것

  

  - << (shift 연산자) : 피 연산자의 비트 열을 왼쪽으로 이동 시킴

    - 1<<n 원소가 n인 경우 n만큼 이동

    

    ```python
    print(1<<3) #output: 8, 1 0 0 0
    print(1<<2) #output: 4, 0 1 0 0
    print(1<<1) #output: 2, 0 0 1 0
    ```

    

  - & (and 연산자) : 피 연산자의 비트 끼리 and 연산

    - 1<<n 원소가 n인 경우 n만큼 이동

    

    ```python
    print(10 & 7)  #output: 2, 1 0 1 0 & 0 1 1 1 -> 2
    ```







- 부분집합, bit로 구현



```python
arr = [1, 2, 3]
n = 3
for i in range(1<<n): #range(8)과 동일한 의미
    # print('i: ', i)
    for j in range(n):
        # print(j) # 0 1 2
        # print(1<<j) # 1 2 4
        # print(i&(1<<j)) # 0 0 0, 1 0 0 , 0 2 0...
        if i&(1<<j): #원소 선택해서 출력
            print(arr[j], end = ' ')
    print()
```





- 부분집합의 합 문제



```python
arr = [-7, -3, -2, 5, 8]
n = len(data)
ans = False #flag #계속 False이다가 조건을 만족하는 어느 순간 True로 바꿔줄 것
for i in range(1<<n):
    sum = 0 #sum의 초기화 위치에 유의
    for j in range(n):
        if i&(i<<j):
            sum += data[j]
    if sum == 0:
        ans = True

print(ans)
```









### 검색

- 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업
- 탐색 키: 자료를 구별하여 인식할 수 있는 키
- 순차 검색, 이진 검색, 해쉬..
- 순서대로 대상과 키 값이 같은 원소가 있는지를 비교하며 찾음 - 찾으면 그 원소의 인덱스 반환 - 자료 구조의 마지막에 이를 때까지 검색 대상을 찾지 못하면 검색 실패





#### 순차 검색

```python
'''
a: 배열
n: 배열의 길이
key: 찾고자 하는 수

배열 안쪽에서, 찾을 때까지만 탐색할 것
'''

def seq_search(a, n, key):

    i = 0 #while에서 쓸 반복 계수 (for문은 없어도 되지만 while에서는 직접 써줘야)
    while i < n and a[i] != key:
        i += 1
    if i < n: return i #검색 성공, 인덱스 리턴
    else: return -1 #구현되어있는 함수의 경우, 검색 실패의 경우 -1을 리턴하는 것이 popular

def seq_search2(a, n, key):
    i = 0
    while i < n and a[i] < key:
        i += 1
    if i < n and a[i] == key : return i
    else: return -1


arr = [4, 9, 11, 23, 2, 19, 7] #정렬이 되어있지 않은 경우, 어레이의 모든 원소를 탐색해야 함
print(seq_search(arr, len(arr), 2))
print(seq_search(arr, len(arr), 8))
arr2 = [1, 2, 3, 4, 5 ,6 ,7] #정렬되어 있는 경우, 검색 가능성에 따라 모든 경우의 수를 탐색할 필요가 없음, 없음을 확신할 수 있음, 수행 속도 빠름
print(seq_search2(arr2, len(arr), 2))
print(seq_search2(arr2, len(arr), 8))
```



- 하나하나 찾아나가는 과정이 썩 간결한 것은 아님





#### 이진 검색 (Binary search)

- 빨라요

- 자료의 가운데에 있는 항목의 키 값과 비교하여 양자 택일, 다음 검색의 위치를 결정하고 검색을 계속 진행

- 이진검색을 하기 위해서는 자료가 정렬된 상태여야

- 중앙의 원소 고르고 - 중앙 원소와 목표 값을 비교 - 목표 값이 작으면 왼쪽에 대해서, 크다면 오른쪽에 대해서 새로 검색을 수행 - 찾을 때까지 반복



- 이진 탐색 구현

```python
'''
검색 시작점, 종료점을 설정함(반복이 진행되면서 시작점(점점 늘어남), 종료점(점점 줄어듦) 갱신)
반복 시작
    시작점이 종료점보다 커진다면 더이상 탐색이 불가하므로 반복을 종료함
    중앙값을 구함
    중앙점의 값과 찾고자 하는 값을 비교
        같으면? 검색 성공
        중앙점의 값이 크면? 왼쪽 탐색 -> 종료점을 갱신
        중앙점의 값이 작으면? 오른쪽 탐색 -> 시작점을 갱신
반복 종료 후 리턴 값이 없다면, 검색 실패이므로 False return
'''

def binary_search(a, key):
    start = 0
    end = len(a) - 1
    while start <= end:
        middle = (start + end)//2
        if a[middle] == key:
            return True
        elif a[middle] > key:
            end = middle - 1 #종료점을 변경하는데, 중앙값의 한 칸 앞으로
        else:
            start = middle + 1

    return False

arr = [2, 4, 7, 9, 11, 19, 23]
print(binary_search(arr, 7))


#재귀로도 구현 가능, 해보기
```







#### 선택 정렬 (Selection sort)

- 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식
- 주어진 리스트 중 최소 값 - 이 값을 맨 앞 위치와 교환 - 맨 처음 위치 제외한 나머지 리스트를 대상으로 위의 과정 반복 - 미정렬원소가 하나 남은 상황에서 마지막 원소가 가장 크므로, 실행 종료



```python
def selection_sort(a):
    for i in range(len(a)-1): #마지막 원소는 비교할 필요X
        min = i #최소값의 인덱스를 저장
        for j in range(i+1, len(a)): #탐색해야 하는 구간 내의 최소값의 인덱스를 찾음
            if a[min] > a[j]:
                min = j
        a[i], a[min] = a[min], a[i] #탐색 구간의 가장 앞과 최소값을 바꿈


arr = [5, 3, 2, 1, 4]
selection_sort(arr)
print(arr)
```

