# 1주차 문제 풀이 인증

## 기본 정보

- 이름: 구혜준
- 목표 문제 수: 3 
- 실제 풀이 문제 수: 3

## 푼 문제 목록

| 번호 | 문제 이름 | 난이도 | 링크 | 풀이 여부 |
| --- | --- | --- | --- | --- |
| 1 | 다음 큰 숫자 | lv2 | https://school.programmers.co.kr/learn/courses/30/lessons/12911 | o |
| 2 | 피보나치 수 | lv2 | https://school.programmers.co.kr/learn/courses/30/lessons/12945 | o |
| 3 | 짝지어 제거하기 | ㅣlv2 | https://school.programmers.co.kr/learn/courses/30/lessons/12973 | o |

---

# 오답노트


## 문제 1

### 문제 정보

- 문제명:다음 큰 숫자
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/12911
- 사용한 알고리즘 / 자료구조: 완전탐색, 이진수 변환

### 처음 접근 방식

이진수로 나타낸 후 for문으로 1의 개수를 세고 count에 저장하여 이 count랑 같은 count가 나오는 값을 반환하도록 했다.

### 풀이 방법

def solution(n):
    
    a = n
    s = bin(n)[2:]
    count_a = 0
    
    for i in s:
        if i == '1':
            count_a += 1
            
    while (True):
        a += 1
        t = bin(a)[2:]
        count_t = 0
        for i in t:
            if i == '1':
                count_t += 1
        if count_a == count_t:
            return a
            break
        
    
    우선 자연수 n의 1의 개수를 구하고, 그 다음으로 a = n으로 설정한 후 1씩 늘려가면서 bin으로 1의 개수를 구하고 만약 n을 이진수로 나타낸 s 의 1의개수 == a를 이진수로 나타낸 t의 1의 개수가 같아진다면 while문을 break으로 끝내고 a값을 return 한다.
    

### 어려웠던 지점

숫자를 이진수로 나타내는 방법을 몰라서 헤맸다. 그리고 while(True)에서 true라고 써서 계속 오류가 났다.

### 해결 방법

bin(숫자)를 하면 0x11111이런식으로 이진수로 변환되어 나타낼 수 있다. 그리고 [2:] 를 하여 앞 0x를 지울 수 있다.

### 시간복잡도 / 공간복잡도

- 시간복잡도:  O(k log n)
- 공간복잡도: O(log n)

### 새롭게 알게 된 점

이진수는 bin(숫자) 로 나타내고 앞 0x를 떼고 싶다면 [2:]로 진행하면 된다.

---

## 문제 2

### 문제 정보

- 문제명: 피보나치 수
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/12945
- 사용한 알고리즘 / 자료구조: 반복문, 동적 계획법

### 처음 접근 방식

    처음엔 for문을 적용하고 일일이 1234567로 숫자들을 나누려고 했다. 그러나 수가 기하급수적으로 커지기때문에 타임아웃이 날 것 같았다.


### 풀이 방법

def solution(n):
    mod = 1234567
    prev = 0
    curr = 1
    
    for i in range(2, n+1):
        curr_mod = (prev + curr) % mod
        prev = curr
        curr = curr_mod
    
    return curr        

    mod를 1234567로 설정하고, 피보나치 수이기 때문에 for문을 반복하되 (curr+prev)%mod 를 먼저 진행한 후 prev = curr, curr 은 curr_mod로 이어나간다. 그리고 curr 현재값을 return 한다.


### 어려웠던 지점

일일이 1234567로 숫자들을 나누려고 했더니 수가 기하급수적으로 커지기때문에 타임아웃이 날 것 같았다. 


### 해결 방법

먼저 나머지를 구한 후 그걸로 curr = curr_mod를 하여 for문을 반복해도 된다는 것을 깨달았다. 그렇게 하여 prev=0 curr=1로 설정하고 curr=curr_mod <- (prev+curr)%mod 한 값 으로 풀었다.


### 시간복잡도 / 공간복잡도

- 시간복잡도: O(n)
- 공간복잡도: O(1)

### 새롭게 알게 된 점

피보나치수열에서 나머지문제가 나오면 모든 반복문에서 일일이 계산해 나머지를 구하기보다는 먼저 mod로 나누고 시작할 것!

---

## 문제 3

### 문제 정보

- 문제명: 짝지어 제거하기
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/12973
- 사용한 알고리즘 / 자료구조: 스택

### 처음 접근 방식

처음에는 stack을 생각하지 못하고, 그 대신 반복문을 떠올렸다. 변수를 지정해서 그걸 가지고 같은 문자가 나왔을 때 +1을 하려는 생각으로 접근했다.



### 풀이 방법

def solution(s):
    stack = []
    
    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else: 
            stack.append(ch)
        
    if not stack:
        return 1
    else:
        return 0
            
    
stack을 선언한 후 stack에 뭔가 들어있고, 또 제일 바깥쪽이 현재 ch 와 같으면 ch를 추가하지 않고 stack에서도 하나 pop. 그렇지 않다면 stack에 ch 하나 넣기


### 어려웠던 지점

변수를 갖고 +1을 하는 방법은 문자가 많기 때문에 불가능하거나 효율적이지 않을 것 같은데, stack이 떠오르지 않아 고민을 많이 했다.


### 해결 방법

stack 을 사용하라는 힌트를 얻고 해당 힌트를 갖고, 문자열이 for문으로 돌아갈 때 하나하나 넣으며 쌓고 앞 문자와 같은 문자가 나왔을 때는 새 문자를 쌓지 않고 앞 문자도 제거하는 방식으로 접근했다.


### 시간복잡도 / 공간복잡도

- 시간복잡도: O(n)  
- 공간복잡도: O(n) 

### 새롭게 알게 된 점

stack을 문자열 문제에서도 잘 활용할 수 있다는 점을 깨달았다. stack에 append로 쌓고 pop으로 빼는 방식을 숙지하고 있어야겠다고 다짐했다. 그리고 stack이 비었을 때 python문법에서는 not stack 이라고 표현하는 것도 알게 됐다.


---

# 다음 주 목표

- 목표 문제 수: 3
- 집중할 유형: Stack / Queue / Hash / 정렬