
## 2020.2.1 TIL


- D2 2005. 파스칼의 삼각형



- 문제 정의 그대로, 두 번째 줄부터 각 숫자들이 자신의 왼쪽과 오른쪽 위의 숫자의 합으로 구성되는 방식을 코드로 작성

  

```python
t = int(input())


for tc in range(1, t+1):
    N = input()
    arr = []

    arr = [[1 for _ in range(row)] for row in range(1, int(N)+1)] #각 행을 1로 채움
    for row in range(int(N)):
        for i in range(1, row): #각 열에 대해 반복

            sum_before = arr[row-1][i]+arr[row-1][i-1] #전 행의 i번째 수와 그 전 수의 합을 변수로 지정
            later = sum_before #이전 행 두 수의 합을 현재 행의 수로 대체
            arr[row][i] = later  # 현재 행의 i번째 수를 변수로 지정

        [print(ele,end=' ') for ele in arr[row][:-1]]
        print(arr[row][-1])


#실행시간: 0.15355s
```





그리고,, 이런 코드를 배웠다. 파이썬,,,,,,, 넘조아



```python
[print(ele,end=' ') for ele in arr[row][:-1]] #1
print(arr[row][-1]) #2
```


#1의 코드로 리스트에서 값만 프린트할 수 있음

#1에서 arr 행의 [:-1] 원소까지 함수처럼 프린트가 호출되어 출력되고

#2에서 행의 마지막 값이 end 조건 후에 출력되어

출력 꼴을 맞춘 파스칼의 삼각형을 만들어냅니다.




- 이전 행에서 현재 행의 왼쪽과 오른쪽에 값을 넘겨주고 합하는 방식으로 코드를 작성
  - 동적프로그래밍을 이용한 방법이라고 자연이 알려줌. 재귀 같음. 실행 시간이 조금 더 빨랐음.



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




#실행시간 0.14438s
```












- D2 1959. 두 개의 숫자열



```python
t = int(input())

for tc in range(1, t + 1):
    n, m = map(int, input().split())
    a_i= list(map(int, input().split(' ')))
    b_i= list(map(int, input().split(' ')))
    l = []

    (long_l, short_l) = (a_i,b_i) if len(a_i)>len(b_i) else (b_i,a_i)

    for k in range(len(long_l) - len(short_l)+1):
        total=0
        for j in range(len(short_l)):
            s = long_l[k+j] * short_l[j]
            total += s
        l.append(total)

    max_value = max(l)

    print(f'#{tc} {max_value}')
```









- D2 1859. 백만장자 프로젝트



```python
def trick(sales):
    if len(sales) in [1,0]:
        return 0

    ans, profit = 0, 0
    max_idx = sales.index(max(sales))

    for sale in sales[:max_idx]:
        ans -= sale

    profit = max_idx * max(sales) + ans

    if max_idx < len(sales)-1:
        profit += trick(sales[max_idx+1:])

    return profit


t = int(input())
for tc in range(1, t+1):
    n = int(input())
    sales = list(map(int, input().split()))

    print(f'#{tc} {trick(sales)}')
```



는 파이참에서는 답이 맞게 나오는데, 재귀로 코드를 짰더니 러닝타임이 너무 길다고....

반복문으로 다시 접근해봐야겠다.









주말 동안 D2 문제 풀이를 모두 완료하는 것을 목표로 종일 알고리즘을 풀었는데

D2 후반부의 문제는 2차원 배열을 이용하는 것이 많아, 묶어서 따로 정리하는 것이 좋겠다.



