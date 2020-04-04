## 2020.2.5 TIL



- flag?
  - flag 조건과 continue를 이용해서 for 문을 계속 돌도록 만드는 방법 

```python
flag = 0

for
for
for 

if flag == 1:
continue
```







- D3 4047. 영준이의 카드게임

```python
t = int(input())
for tc in range(1, t+1):
    s = input()
    cards = {'S': 13, 'D': 13, 'H': 13, 'C': 13}

    pattern_info = [s[i] for i in range(len(s)) if i % 3 == 0]
    number_info = [s[i+1:i+3] for i in range(len(s)) if i % 3 == 0]
    error_info = [s[i:i+3] for i in range(len(s)) if i % 3 == 0]

    for i in range(len(pattern_info)):
        for key in cards:
            if pattern_info[i] in key:
                cards[pattern_info[i]] -= 1

    if len(set(error_info)) < len(error_info):
        print('#{} ERROR'.format(tc))

    else:
        print('#{} {} {} {} {}'.format(tc, cards['S'], cards['D'], cards['H'], cards['C']))
```





- 이차원 배열을 이용한 풀이도 가능 (열 0~13, 행 s d h c)



```python
t = int(input())
for tc in range(1, t+1):
    deck = [[0 for i in range(13)] for j in range(4)]
    types = ['S', 'D', 'H', 'C']
    cards = list(input())
    isError = False

    while cards and not isError:
        type = cards.pop(0)
        nums = cards.pop(0)
        nums = nums + cards.pop(0)
        num = int(nums)

        for i in range(len(types)):
            if type == types[i]:
                if deck[i][num] == 0: #아직 카드가 없다면
                    deck[i][num] =1 #1로 채움
                else:
                    isError = True
                    break
    print('#{}'.format(tc), end=' ')
    if isError:
        print('ERROR')
    else:
        for row in deck:
            need = 0
            for c in row:
                if c == 0: #카드가 없으니 니즈에 1을 더함
                    need += 1
            print(need, end = ' ')
        print()
```







- D3 4751. 다솔이의 다이아몬드 장식



```python
t = int(input())
for tc in range(1, t+1):
    s = input()
    n = len(s)

    l_3 = ['.' for _ in range(4*(n-1)+5)]
    for i in range(n):
        l_3[4 * i + 2] = s[i]
    for i in range(n + 1):
        l_3[4 * i] = '#'

    l_2 = ['.' for _ in range(4*(n-1)+5)]
    for i in range(len(l_2)):
        if i % 2:
            l_2[i] = '#'

    l_1 = ['.' for _ in range(4*(n-1)+5)]
    for i in range(n):
        l_1[4 * i + 2] = '#'

    print(''.join(l_1))
    print(''.join(l_2))
    print(''.join(l_3))
    print(''.join(l_2))
    print(''.join(l_1))
```

- '만약 문자열의 길이가 1보다 더 크면, 인접한 문자는 ‘#’과 ‘.’을 공유하여 장식한다'에 유의하여
  4*n +1의 꼴을 이용







- D3 6190. 정곤이의 단조 증가하는 수

- 문자열 다루는 것은 실행에서 소요시간이 기니,
  정수형에서 계산 끝낼 수 있으면 정수형 다루는 것 권장



```python
123 -> 큰 자리 -> 작은자리 : 증가		-> 단조증가
123 -> 작은 자리 -> 큰 자리 : 감소		-> 단조증가

123 % 10 -> 3 : 1의 자리
123 // 10 -> 12: 1의 자리는 사라짐

12 % 10 -> 2 : 10의 자리
12 // 10 -> 1 : 10의 자리는 사라짐

1 % 10 -> 1 : 100의자리
```



```python
t = int(input())
for tc in range(1, t+1):
    n = int(input())
    arr = list(map(int, input().split()))
    maxV = -1 #최대값을 찾기 위한 변수 (최대값을 아예 찾을 수 없을 경우, -1반환이 문제의 질문이니)

    for i in range(len(arr)):
        for j in range(len(arr)):
            num = arr[i]*arr[j]
            if i < j and maxV < num: #최소값이 아닌 수는 단조증가하는지 체크할 이유 없음, 따라서 지금까지 찾은 맥시멈 값의 조건을 넣어줌
                num_copy = num #원본 숫자를 조작할 것이니, 백업 용도
                pre = 10 #하나의 자리 수에 올 수 있는 최대값이 9이므로, 10으로 초기화함
                isInc = True #증가 하는지를 판단하기 위한 분리값

                while num:
                    n = num % 10 #일의자리 수 얻음
                    if pre < n: #n이 이전 수보다 크면 -> 단조증가가 아님
                        isInc = False
                        break
                    num = num//10
                    pre = n
                #최대값 찾기
                if isInc: #단조 증가 할 때만
                    if maxV < num_copy:
                        maxV = num_copy

    print('#{} {}'.format(tc, maxV))
```



- 단조 증가 평가 위한 반복문 아이디어

```python
                while num:
                    n = num % 10 #일의자리 수 얻음
                    if pre < n: #n이 이전 수보다 크면 -> 단조증가가 아님
                        isInc = False
                        break
                    num = num//10
                    pre = n
```



[파이썬 곱 함수는 왜 없는가](https://shoark7.github.io/programming/algorithm/3ways-to-get-multiplication-in-a-list-in-python)