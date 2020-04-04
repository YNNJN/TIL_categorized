## 2020. 2. 2 TIL




회문의 날!

D2 문제 후반부의 2차원 배열을 이용한 문제들을 정리하려 한다.

해당되는 문제는 1961번 숫자 배열 회전 문제, 1954번 달팽이 숫자 문제, 1974번 스도쿠 검증 문제, 1979번 빈칸에 단어 넣는 문제, 2001번 파리 퇴치 문제, 2005번 파스칼의 삼각형 문제 정도인듯.









먼저 2차원 배열을 입력받는 방법은 다음과 같다.



#### 2차원 배열 입력 받기

```python
#2차원 배열 입력 받기
# 3행
# 123
# 456
# 789

N = int(input())
arr1 = [0 for _ in range(N)]     
arr2 = [list(map(int, input().split())) for _ in range(N)]
print(arr1)
print(arr2)
```



#### N번만큼 앞의 내용을 반복

```python
for _ in range(N)		# _ : 반복계수가 필요 없음을 의미. 0을 N번 반복하여 리스트에 넣음
```



#### 0을 M만큼 리스트에 담은 것을 N번 반복

```python
room = [[0 for _ in range(M)] for _ in range(N)]]
```











- D2 1961. 숫자 배열 회전 문제 



```python
def rotate(arr):
    n = len(arr)
    arr_copy = [[0 for i in range(n)] for j in range(n)]

    for i in range(len(arr)):
        for j in range(len(arr)):

            arr_copy[j][n-1-i] = arr[i][j]

    return arr_copy


t = int(input())

for tc in range(1, t+1):
    print(f'#{tc}')
    n = int(input())
    arr = [list(map(int, input().split())) for i in range(n)]


    rotate_90 = rotate(arr)
    rotate_180 = rotate(rotate_90)
    rotate_270 = rotate(rotate_180)


    for p in range(n):
        tmp=rotate_90[p]+['-']+rotate_180[p]+['-']+rotate_270[p]
        for ele in tmp:
            if ele=='-': print(' ',end='')
            else : print(ele,end='')
        print()
```



- 아주 매뉴얼하게 인풋을 받고, 0으로 채운 정사각 배열을
  아래의 아이디어로 90도 회전하는 함수를 구현하여 문제를 풀이함

```python
arr_copy[j][n-1-i] = arr[i][j]
```



- 출력 형태를 맞추기 위해
  tmp에서 출력 요소를 모두 꺼내고, 이를 replace 하는 방식으로 조작하는 아이디어 또한 이용함











- D2 1954. 달팽이 숫자 문제



```python
t = int(input())

for tc in range(1, t+1):
    n = int(input())
    arr = [[0 for i in range(n)] for j in range(n)]
    i,j = (0,0)
    direc = ['r','d','l','u']
    direc_idx = 0

    for num in range(1,n*n+1):
        arr[i][j]=num

        current_direc = direc[direc_idx]
        if current_direc == 'r':
            if j+1 == n or arr[i][j+1] != 0 :
                direc_idx += 1
                direc_idx = direc_idx%4
                i += 1
            else :
                j += 1
        elif current_direc =='d':
            if i+1==n or arr[i+1][j]!= 0 :
                direc_idx += 1
                direc_idx = direc_idx%4
                j -= 1
            else :
                i += 1
        elif current_direc == 'l':
            if j==0 or arr[i][j-1]!=0 :
                direc_idx += 1
                direc_idx = direc_idx%4
                i -= 1
            else :
                j -= 1
        else :
            if arr[i-1][j]!=0:
                direc_idx += 1
                direc_idx = direc_idx % 4
                j += 1
            else:
                i -= 1

    print(f'#{tc}')
    for ele in arr:
        for el in ele[:-1]:
            print(el, end = ' ')
        print(ele[-1])
```



- 0으로 n*n 배열을 채우고 현재 이동 중인 방향에 따라
  i, j 중 어떤 것을 증가시킬지, 혹은 증가시켜도 되는지(모서리에 다다르거나, 이미 채워진 숫자가 있는지가 조건)에 대한 여부를 판단하는 아이디어로 문제를 풀이함

  

- 게임 만드는 것 같았다 하하호











- D2 1974. 스도쿠 검증 문제



```python
def sudoku_tf(arr):
    standard = set(range(1, 10))

    for i in range(9):
        temp = set(arr[i])
        if temp == standard:
            pass
        else:
            return 0

    for j in range(9):
        temp = set()
        for i in range(9):
            temp.add(arr[i][j])
        if temp == standard:
            pass
        else :
            return 0


    for k in range(3):
        for m in range(3):
            temp = set()
            for ele in arr[3*k:3*k+3]:
                for el in ele[3*m:3*m+3]:
                    temp.add(el)
            if temp != standard:
                return 0
    return 1


t = int(input())

for tc in range(1, t+1):
    arr = [list(map(int, input().split())) for i in range(9)]

    print(f'#{tc} {sudoku_tf(arr)}')
```



- 행 별, 열 별, 정사각 구간 별로 스도쿠 여부를 판단하는 함수를 구현하여 문제를 풀이함
  특히 정사각 구간 별로 포문의 중첩으로 슬라이싱 하는 아이디어로 문제를 풀이함

```python
for k in range(3):
    for m in range(3):
        temp = set()
        for ele in arr[3*k:3*k+3]:
            for el in ele[3*m:3*m+3]:
                temp.add(el)
        if temp != standard:
            return 0
```


- return 0인 조건들을 걸러내는 로직을 먼저 시도했다면 더 수월했을 것











- D2 1979. 어디에 단어가 들어갈 수 있을까 문제



```python
t = int(input())

for tc in range(1, t+1):
    n, k = map(int, input().split())
    puzzle = [list(map(int, input().split())) for _ in range(n)]
    case = 0

    for i in range(n):
        row_count = 0
        for c in range(n):
            if puzzle[i][c]:
                row_count+=1
            else :
                if row_count==k:
                    case+=1
                row_count = 0
        if row_count == k:
            case += 1

    for j in range(n):
        col_count = 0
        for r in range(n):
            if puzzle[r][j]:
                col_count+=1
            else :
                if col_count==k:
                    case+=1
                    print(r,j,case)
                col_count = 0
        if col_count == k:
            case += 1
    print(f'#{tc} {case}')
```



- row와 column에 대해서 각각을 탐색하는 방식을 매뉴얼하게 구현함
  1이 연속해서 k개 있으면 경우의 수에 1을 더하는 방식. 초기화 또한 고려해야 했음











- D2 2001. 파리 퇴치 문제



```python
t = int(input())

for tc in range(1, t+1):
    n, m = map(int, input().split())
    arr = [list(map(int, input().split())) for _ in range(n)]
    max_value = 0
    temp_sum = []

    for i in range(n-m+1):
        for j in range(n-m+1):
            part_sum = 0
            for k in range(m):
                part_sum += sum(arr[i+k][j:j+m])

            temp_sum.append(part_sum)
    max_value = max(temp_sum)


    print(f'#{tc} {max_value}')
```



- for문 세 개의 중첩을 통해 n-m+1의 i, j의 범위를 지정하고
  2차원 배열의 행(리스트)을 슬라이싱 하여 구간을 설정하는 아이디어로 문제를 풀이함

```python
for i in range(n-m+1):
    for j in range(n-m+1):
        part_sum = 0
        for k in range(m):
            part_sum += sum(arr[i+k][j:j+m])
```










- D2 2005. 파스칼의 삼각형 문제



```python
t = int(input())

for tc in range(1, t+1):
    N = input()

    pre_row=[1]
    print(1)
    for row in range(1,int(N)):
        post_row = [ele for ele in pre_row] # 이전 행을 복사 :  바로 위에서 덧셈

        post_row.append(0)

        for i in range(len(pre_row)):
            post_row[i+1] += pre_row[i] #이전 칸의 덧셈

        pre_row=post_row #새로운 칸

        [print(ele,end=' ') for ele in post_row[:-1]]
        print(post_row[-1])
```



- 이전 행에서 현재 행의 왼쪽과 오른쪽에 값을 넘겨주고 합하는 방법으로 문제를 풀이함





