## 2020. 2. 13 TIL



## Memoization



- 피보나치 수를 재귀함수로 구현한 알고리즘 -> 중복 호출 아주 많음
- O(2^n)인 call Tree 참고, 
- 같은 함수를 여러 번 계산할 이유가 없으니, 이미 계산한 값을 메모리에 저장해두자
- 즉 메모이제이션을 이용해 계산 횟수를 줄일 수 있음



#### 정리

- 메모이제이션: 연산 결과를 저장 -> 중복 계산을 하지 않음
- 방법은 메모 저장을 위한 배열을 할당하고 0으로 초기화 
- 실행 시간을 O(2^n) -> O(n)으로 줄일 수 있음



```python
'''재귀함수 피보나치'''
def fibo(n):
    if n < 2:
        return n
    return fibo(n-1) +fibo(n-2)
print(fibo(5))


'''메모이제이션 이용'''
def fibo1(n):
    pass
    #n이 2보다 같거나 크고 n이 메모 길이보다 길면, 즉 아직 계산되지 않았다면
    if n >= 2 and len(memo) <= n:
        memo.append(fibo1(n-1) + fibo1(n-2)) #계산을 하고 메모이제이션 배열에 저장함
    return memo[n] #아니라면, 즉 이미 계산된 값이면, 메모이제이션 배열에 저장된 값을 리턴

memo = [0,1] #메모이제이션 초기화

print(fibo1(5))
```









## DP



- Dynamic Programming
- 그리디알고리즘과 같이 최적화 문제를 해결하는 알고리즘
- 방법은 입력 크기가 작은 부분을 모두 해결하고, 그 해를 이용하여 큰 크기 부분의 문제를 해결



- 피보나치는 최적 부분구조로 이루어져 있기 때문에 DP 적용 가능
- 최적 부분구조: 부분 문제의 답으로부터 본 문제의 답을 얻는 꼴

```python
#DP 적용
#(재귀와의 큰 차이점은, 사람이 계산하는 것처럼 계산함)
#반복으로 구현

def fibo2(n):
    f = [0,1] #초기화
    for i in range(2, n+1): #f[2] -> f[n]까지 답을 구함
        f.append(f[i-1] + f[i-2])
    return f[n]

print(fibo2(5))
```



- 메모이제이션~재귀, DP~반복

- 재귀는 컴퓨터의 스택 메모리 이용, 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생함
- 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적









## 그래프



#### 특성

- 비선형 자료구조
- 그래프 G는 (V,E)의 쌍
- V: 정점의 집합 1,2,3(독립적)
  - 정점은 독립된 개체로 동그라미로 표현
- E: 간선의 집합 (시작과 끝점을 (u,v)로 표현, (1,2), (2,3), (1,3))
  - 간선은 두 정점을 이어주는 개체로 선(무방향그래프)이나 화살표가 있는 선(방향 그래프)으로 표현
  - 간선은 좌표 개념이 아님에 유의, 연결 여부만 표시

- 인접: u,v라는 간선이 있다면, 정점 u와 정점 v가 서로 인접하다(연결되어있음)





#### 표현

- 인접 행렬로 표현 (간선 번호 개수만큼의 크기)

  - 크기가 V * V인 행렬
  - 두 정점 i,j를 잇는 간선이 있다면, 인접 행렬의 i,j는 1, 아니면 0

- 무방향 그래프

  - 양방향으로 간선이 존재한다는 의미이므로 대칭행렬이 됨

- 방향 그래프

  - 대칭이 아님

  



- 인접 행렬을 생성하고 출력(무방향그래프)

```python
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''

V,E = map(int,input().split())

#인접행렬
adj = [[0 for _ in range(V+1)] for _ in range(V+1)]
edge = list(map(int,input().split()))

for i in range(E):
    n1,n2 = edge[i*2],edge[i*2+1]
    adj[n1][n2] = 1         #무방향그래프이므로 둘다 1, 대칭행렬이 만들어짐
    adj[n2][n1] = 1
for row in adj:
    print(row)
```









## DFS



#### 특성

- 그래프는 비선형 자료구조
- DFS는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 데 사용하는 방법
- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해가다가 더 이상 갈 곳이 없게 되면,
  가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서(이 때 스택 이용 구현 가능)
  다른 방향 정점으로 탐색을 계속 반복





#### 방법

- 시작 정점 v결정, 방문
- v와 인접 정점 중 방문하지 않은 정점w가 있으면 v를 push하고 정점 w를 방문
- w를 v로 하여 다시 바로 위의 과정을 반복
- 방문 안 한 정점이 없다면 탐색 방향 바꾸기 위해 스택을 pop,가장 마지막 방문 정점을 v로 하여 위위의 과정 반복
- 스택이 공백이 될 때까지





#### 구현

- 배열 visited는 정점 크기만큼의 배열
- visited를 False로 초기화하고, 공백 스택 생성
- 정점에 방문하지 않은 인접한 정점이 있으면, 원 정점을 스택에 push, 그리고 다른 정점 중 오름차순에 따라 선택하여 탐색을 계속함
- 인접한 곳 중 방문 안 한 인접정점이 없다면, 스택에서 pop하여 이전으로 되돌아감
- 스택이 공백이면 DFS를 종료함





- stack을 이용하여 DFS 구현

```python
'''
스택을 활용한 DFS 구현
    방문배열, 스택 준비

    시작점을 스택에 넣고 방문 표시함
    스택이 빌 때까지 반복
        스택에서 정점을 꺼내옴
        출력
        꺼내온 정점에 인접하고 또 아직 방문하지 않은 정점을 스택에 넣음
        스택에 넣은 정점을 방문 표시
'''


def dfs(n,V):
    visited = [0] * (V+1) #1차원은 괜찮, 2차원에서 이 방식으로 초기화하면 얕은 복사가 됨
    stack = [0] * V
    top = -1

    top += 1
    stack[top] = n #시작점을 스택에 넣음
    visited[n] = 1 #방문했음을 1로 표시

    while top >= 0:
        n = stack[top] #스택에서 하나 꺼내옴
        top -= 1
        print(n, end='')
        for i in range(1, V+1): #행의 인덱스는 정점, 그것이 어떤 정점과 연결되어있는지
            if adj[n][i] == 1 and visited[i] == 0:
                top += 1
                stack[top] = i
                visited[i] = 1

V,E = map(int, input().split())
adj = [[0 for _ in range(V+1)] for _ in range(V+1)]
edge = list(map(int, input().split()))
for i in range(E):
    n1, n2 = edge[i*2], edge[i*2+1]
    adj[n1][n2] = 1
    adj[n2][n1] = 1
for row in adj:
    print(row)

dfs(1,V) #시작 정점, 정점 개수
```



```
input
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
output
1376542 (찾아들어가는 경로 의미, 한 번 방향을 정하면 끝까지 찾아들어감)
```







- D4 1226. 미로1



- DFS를 어떻게 적용해야할지 감을 못잡아서,, 그냥 사방 탐색으로만 풀었다



```python
def search(r, c, board):
    global flag
    if flag:
        return

    dr = [0, 0, 1, -1]
    dc = [1, -1, 0, 0]

    board[r][c] = 1
    for i in range(4):
        tr = r + dr[i]
        tc = c + dc[i]

        if not (0 <= tr < 16 and 0 <= tc < 16):
            continue

        if board[tr][tc] == 3:
            flag = True
            return
        elif board[tr][tc] == 1:
            continue
        search(tr, tc, board)


for tc in range(1, 11):
    t = input()
    board = [list(map(int, input())) for _ in range(16)]
    flag = False
    search(1, 1, board)

    print('#{}'.format(tc), end=' ')
    if flag:
        print(1)
    else:
        print(0)
```



- 그리고 방문 배열 만들고, 스택 이용한 DFS로도 구현 가능한 방법을 드디어 배움



```python
def find(sr, sc):
    dr = [-1, 0, 1, 0]
    dc = [0, 1, 0, -1]
    s = [] #stack
    visited = [[0 for _ in range(n)] for _ in range(n)]
    s.append([sr, sc]) #시작점을 스택에 넣음
    visited[sr][sc] = 1 #시작점을 방문했다고 표시함
    while s: #스택이 비어있으면 False를 리턴하니, 스택에 자료가 있는 동안을 의미
        cur = s.pop() #스택에서 하나 꺼내옴 -> 현재위치
        for k in range(4):
            nr = cur[0] + dr[k]
            nc = cur[1] + dc[k]

            #인덱스가 범위 안인지 검사할 이유 없음, 네 변이 모두 1이니

            if maze[nr][nc] == 3:
                return True
            elif maze[nr][nc] == 0 and visited[nr][nc] == 0: #새로운 좌표가 길이고 아직 방문 전일 때
                s.append([nr, nc])
                visited[nr][nc] = 1 #방문했음을 표시
    return False

t,n = 10,16
for tc in range(1, t+1):
    tc = int(input())
    maze = [list(map(int, input())) for _ in range(n)]
    result = 0
    for i in range(n):
        for j in range(n):
            if maze[i][j] == 2:
                if find(i,j): #탐색하고 결과가 True이면
                    result = 1
    print('#{} {}'.format(tc, result))
```





- 스택을 이용한 dfs 구현 관련 아이디어 정리

스택에서 밑바닥에 있는 값을 꺼내온다 = 안 가봤던 길을 간다

미로, 선의 개수를 찾아라,,, 등의 문제에 적용함

못 찾으면 무엇으로 표현하라 -> 초기화를 그 값으로 해두고, 그 값을 갱신할 기회를 주지 않는 방식으로 접근

