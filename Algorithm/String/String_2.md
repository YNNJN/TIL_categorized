## 2020. 2. 11 TIL



- D3 4864. 문자열 비교



```python
t = int(input())
for tc in range(1, t+1):
    str1 = input()
    str2 = input()

    i, j = 0, 0
    while j < len(str1) and i < len(str2):
        if str1[j] != str2[i]:
            i = i-j
            j = -1
        i += 1
        j += 1
        if j == len(str1):
            print('#{} {}'.format(tc, 1))
            break
    else:
        print('#{} {}'.format(tc, 0))
```



- 반복문의 중첩을 이용한 함수로 풀이도 가능



```python
def find():
    for i in range(len(str2) - len(str1) + 1):
        j = 0
        while str1[j] == str2[i+j]:
            j += 1
            if j == len(str1):
                return 1
    return 0
```







- D3 4865. 글자 수



- 아래는 아주 매뉴얼한 방식으로 구현한 것



```python
t = int(input())
for tc in range(1, t+1):
    str1 = input()
    str2 = input()

    alpha1, alpha2 = {}, {}
    max_value = 0

    for i in range(len(str1)):
        if str1[i] in alpha1:
            alpha1[str1[i]] += 1
        else:
            alpha1[str1[i]] = 1

    for i in range(len(str2)):
        if str2[i] in alpha2:
            alpha2[str2[i]] += 1
        else:
            alpha2[str2[i]] = 1

    l = []
    for key in alpha1:
        if key in alpha2:
            l.append(key)

    for el in l:
        if el in alpha2:
            if max_value < alpha2[el]:
                max_value = alpha2[el]

    print('#{} {}'.format(tc, max_value))
```



- {}.fromkeys() 함수 이용한 풀이도 가능



```python
t = int(input())
for tc in range(1, t+1):
    str1 = input()
    str2 = input()

    count = {}.fromkeys(str1, 0) #str1 문자열을 이용해서 키 생성, 밸류는 0, 타입은 딕셔너리
    # print(count)
    for ch in str2:
        if ch in count:
            count[ch] += 1
    print('#{} {}'.format(tc, max(count.values())))
```







- D3 4861. 회문



- flag도 써보고,,, 세로 방향으로 읽을 땐 값으로 접근해야하니 cnt 쓰고 idx 저장해오면서 아주 매뉴얼하게 구현함
- 아래 코드 마지막 줄의 인덱스 보정을 위한 디버깅에 굉장한 에너지를 소비했,,,, 지만 구현 성공했다



```python
        for j in range(n):
            for k in range(n-m+1):
                cnt = 0
                for i in range(m):
                        if word[k+i][j] == word[k+m-i-1][j]:
```



- 전체 코드는 아래와 같음



```python
t = int(input())
for tc in range(1, t+1):
    n,m = map(int, input().split())
    word = [list(input()) for _ in range(n)]

    flag = 1
    if flag:
        new_word = []
        for i in range(n):
            for k in range(n-m+1):
                new_word.append(word[i][k:k+m])

        for i in range(len(new_word)):
            for k in range(m):
                if new_word[i][k] != new_word[i][-k-1]:
                    break
            else:
                print('#{} {}'.format(tc, ''.join(new_word[i])))
                flag = 0

    if flag:
        l = []
        idx, cnt = 0, 0
        for j in range(n):
            for k in range(n-m+1):
                cnt = 0
                for i in range(m):
                        if word[k+i][j] == word[k+m-i-1][j]:
                            cnt += 1
                            if cnt == m:
                                idx = j
                                idx2 = k

        for i in range(idx2, idx2+m):
            l.append(word[i][idx])

        print('#{} {}'.format(tc, ''.join(map(str, l))))
```





- 함수로 잘 정리하면 아래의 코드가 가능하겠고



```python
def find():
    for i in range(N):
        for j in range(N-M+1):
            k = 0
            h = M//2 #비교 횟수
            while k < h:
                if s[i][j+k] != s[i][j+M-k-1]:
                    break
                k+=1
            if k == h:  #회문 찾음
                print(s[i][j:j+M])
                return  #회문은 1개니까 찾으면 바로 리턴
            k = 0
            while k <h:
                if s[j+k][i] != s[j+M-k-1][i]:
                    break
                k+=1
            if k == h:
                for ii in range(j,j+M):
                    print(s[ii][i], end='')
                print()
                return


T = int(input())
for tc in range(1,T+1):
    N,M = map(int,input().split())
    s = [input() for i in range(N)]     #문자열 입력받아서 2차원으로
    print("#{}".format(tc),end=' ')
    find()
```





- D3 1215. 회문1



- 완전히 같은 내용의 문제인데
  슬라이싱 이용해서 더 간단하게 구현함



```python
def is_palindrome(a):
    for i in range(8):
        if a != a[::-1]:
            return False
    return True

for tc in range(1, 11):
    m = int(input())
    arr = [list(input()) for _ in range(8)]
    cnt = 0

    for i in range(8):
        for k in range(8-m+1):
            if is_palindrome(arr[i][k:k+m]):
                cnt += 1

    for j in range(8):
        for k in range(8-m+1):
            temp =''
            for i in range(k, k+m):
                temp += arr[i][j]
            if is_palindrome(temp):
                cnt += 1

    print('#{} {}'.format(tc, cnt))
```



- 혹은 원본과 열 우선순위로 정렬한 사본을 원본 아래에 두어, 한꺼번에 탐색하는 방법으로도 풀이가 가능함
- 또한 스택을 이용한 방법
  절반은 집어넣어놓고 나머지는 꺼내오면서 비교



```python
def find():
    global cnt
    s = [] #stack
    for i in range(16):
        for j in range(8-n+1): #탐색 시작점 설정
            len = 0 #회문 길이 저장
            half = n//2 if n%2 ==0 else n//2+1
            for k in range(half): #회문의 반을 s에 담음
                s.append(map[i][j+k])
            if n%2: #홀수면 자기 자신 버림(회문 판별에 영향 x)
                s.pop()
                len = 1
            for k in range(n//2):
                if s.pop() == map[i][j+half+k]: #half만큼 offset
                    len += 1
            if len == half:
                cnt += 1
```







- D3 1216. 회문2



- arr의 행과 열을 바꾼 new_arr행을 지정하여, 세로 탐색을 따로 두지 않고 가로 탐색을 똑같이 적용한 문제 풀이도 가능함



```python
def is_palindrome(a):
    if a != a[::-1]:
        return False
    return True

def find_max(arr):
    m = 100
    while m > 0:
        for i in range(100):
            for k in range(100 - m + 1):
                if is_palindrome(arr[i][k:k + m]):
                    return m
        m -= 1

for tc in range(1, 11):
    t = int(input())
    arr = [list(input()) for _ in range(100)]

    max_len_row = find_max(arr)

    new_arr = [[0 for _ in range(100)] for _ in range(100)]
    for j in range(100):
        for i in range(100):
            new_arr[j][i] = arr[i][j]

    max_len_col = find_max(new_arr)

    max_len = max(max_len_col, max_len_row)
    print('#{} {}'.format(tc, max_len))
```







- 탐색 추가 연습



- 마지막에 ty와 tx의 순서를 바꿔주는 부분에서 많이 헷갈림
  x, y 대신 r,c 혹은 i, j 쓰도록 해야지



```python
def bloom(n,m,arr):
    dx = [-1, 0, 1, 0]
    dy = [0, -1, 0, 1]

    result = 0
    for i in range(n):
        for j in range(m):
            s = arr[i][j]
            for k in range(1, s+1):
                for p in range(4):
                    tx = j + k*dx[p]
                    ty = i + k*dy[p]

                    if tx >= 0 and ty >= 0 and tx < m and ty < n:
                        s += arr[ty][tx]
                        # print(tx, ty, arr[tx][ty], s)
                    if result < s:
                        result = s
    return result

t = int(input())
for tc in range(1, t+1):
    n,m = map(int, input().split())
    arr = [list(map(int, input().split())) for _ in range(n)]

    print('#{} {}'.format(tc, bloom(n,m,arr)))
```

