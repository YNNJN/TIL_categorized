## 2020.2.7 TIL



- D3 1493. 새로운 연산

```python
grid = [[]]
d = 1 #대각선의 길이
x, y = 1, 1
m = 1
while m <= 10000:
    for x in range(1,d+1):
        y = d - x+1
        grid.append([x,y])
        m += 1
    d += 1
print(grid)
```



- 저걸 만들고 싶어서 내가 시도했던 방법들은
for문의 작동 순서를 고려하지 못했었음



- M1

```python
flow_num = []
for i in range(1, 300):
    for j in range(1, 300):
        if j + (i-j) == i:
            flow_num += [[i,j]]
print(flow_num)
```



- M2

```python
flow_num = []
for i in range(1, 9):
	for j in range(1, 9):
		k = 2
		while k != 9:
			if k == i+j:
				flow_num += [[i,j]]
			k += 1
print(flow_num)
```



- M3 (계차수열)

```python
flow_num = []
for i in range(300):
	for j in range(300):
		flow_num.append(1 + int((j*(j-1))/2))
```



- 1~x'의 행에 쓰여있는 값
  - (x'+1)x'/2
- x'의 대각선에 속한 숫자 중  행에 쓰여있는 값
  - (x'+1)x'/2 -y +1
    - x' = x + y - 1 를 대입하여 계산하면 값을 얻을 수 있음
    - 즉, x와 y의 좌표를 알면 대각선의 수의 계산이 가능함



```python
grid = [[]]
d = 1 #대각선의 길이
x, y = 1, 1
m = 1
while m <= 10000:
    for x in range(1,d+1):
        y = d - x+1
        grid.append([x,y])
        m += 1
    d += 1
print(grid)

t = int(input())
for tc in range(1, t+1):
    p,q = map(int, input().split())
    #1 star 5 = #(&(1) + &(5))
    # &(1) -> (1,1), &(5) -> (2,2)
    # #(1, 1) + #(2, 2) = #(3, 3)
    x = grid[p][0] + grid[q][0]
    y = gird[p][1] + gird[q][1]

    n = (x+y+1)*(x+y)//2 -y +1
    print('#{} {}'.format(tc, n))
```





- 이차원 배열에 모든 값을 만들고 찾아서 풀어도 됨
  위의 방법 생각하느라 이 접근을 생각 못했다
- 완전탐색부터 시도하세여



```python
for tc in range(int(input())):
    p, q = map(int, input().split())
    mapping = [[0 for i in range(300)] for j in range(300)]
    a = 1
    p_x, p_y, q_x, q_y = 0, 0, 0, 0

    '''
    (1, 1), (1, 2), (2, 1), (1, 3), (2, 2), (3, 1)....
    대각선 순서로 점에 수를 붙인 리스트 -> 인덱스의 합이 같은 것끼리 정렬된 리스트
    그리하여 mapping[i - j][j] = a, 즉 i를 합으로 하는 (i-j, j)의 원소라는 아이디어를 적용 가능
    해당 리스트는 합이 2부터 시작하므로 a는 1로 초기화
    
    '''

    for i in range(599):
        for j in range(300):
            if 300 > i-j >= 0:
                mapping[i-j][j] = a #i를 합으로 하는 (i-j, j)에 대해
                a += 1

    for i in range(300):
        if p in mapping[i]:
            p_y = mapping[i].index(p)
            p_x = i

    for i in range(300):
        if q in mapping[i]:
            q_y = mapping[i].index(q)
            q_x = i

    result = mapping[p_x + q_x + 1][p_y + q_y + 1]
    print(f'#{tc + 1} {result}')

```



