# N주차 문제 풀이 인증

## 기본 정보

- 이름: 이윤서
- 목표 문제 수: 3
- 실제 풀이 문제 수: 3

## 푼 문제 목록

| 번호 | 문제 이름 | 난이도 | 링크 | 풀이 여부 |
| --- | --- | --- | --- | --- |
| 1 | 올바른 괄호 | Lv.2 | https://school.programmers.co.kr/learn/courses/30/lessons/12909 | O |
| 2 | 기능개발 | Lv.2 | https://school.programmers.co.kr/learn/courses/30/lessons/42586 | O |
| 3 | 전화번호 목록 | Lv.2 | https://school.programmers.co.kr/learn/courses/30/lessons/42577 | O |

---

# 오답노트

## 문제 1

### 문제 정보

- 문제명: 올바른 괄호
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/12909
- 사용한 알고리즘 / 자료구조: 스택

### 처음 접근 방식

'('을 만나면 push, ')'을 만나면 pop. 반복문 끝나고 스택에 값이 있으면 false, 아니면 true.

### 풀이 방법

```python
def solution(s):
    stack = []
    for i in s:
        if i =='(':
            stack.append(i)
        elif i == ')':
            if len(stack) == 0:
                return False
            else:
                stack.pop()
    if len(stack) > 0:
        return False
    else:
        return True
```

* 리스트의 길이가 1이상이면 에러
* pop했는데 에러이면 에러
* 이거 둘 다 아니면 true

### 어려웠던 지점

* for문에서 in 뒤에는 이터러블 객체가 와야함. 평소에 range()하는 이유는 range 자체가 이터러블 함수라서 숫자를 준다면 그걸 반환해주는 것
* 파이썬에서 스택 사용법: stack을 리스트로 선언. 리스트의 append, pop 사용
* 길이 찾는 것 a.length가 아니라 len(a) !
* return 하는 것 주의 (조건문 양쪽 다 return 안 해주면 None 반환됨)

### 해결 방법

pop 하기 전에 스택이 비어있는지 먼저 확인하는 순서로 고침. `range(s)` → `for i in s:`, `.len` → `len()`으로 수정.

### 시간복잡도 / 공간복잡도

- 시간복잡도: O(n)
- 공간복잡도: O(n)

### 새롭게 알게 된 점

에러가 날 상황을 미리 조건문으로 막는 게 try-except보다 깔끔할 때가 있음.

---

## 문제 2

### 문제 정보

- 문제명: 기능개발
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/42586
- 사용한 알고리즘 / 자료구조: 큐 (그룹 짓기 로직)

### 처음 접근 방식

각 기능마다 완성까지 며칠 걸리는지 먼저 계산. 완성일수 = (100 - 진도) / 속도.

### 풀이 방법

* 지금 보고 있는 기능의 완성일 > 그룹의 첫 번째 기능의 완성일이면: 새로운 배포 그룹 시작
* 아니면: 같은 그룹으로 묶여서 같이 배포됨 (같이 배포되어야 하니까)
* 그룹이 바뀔 때마다 이전 그룹의 개수를 answer에 저장

```python
import math
def solution(progresses, speeds):
    answer = []
    days = []
    for i in range(len(progresses)):
        remain = 100 - progresses[i]
        day = math.ceil(remain / speeds[i])
        days.append(day)

    first = days[0]
    count = 0

    for d in days:
        if d <= first:
            count += 1
        else:
            answer.append(count)
            first = d
            count = 1

    answer.append(count)
    return answer
```

### 어려웠던 지점

* 리스트로 구현할 시: append, pop[0] 을 이용 / 큐 라이브러리 이용할 시 `import queue` → `queue.Queue()` → `q.put(a)` / `q.get()` / `q.qsize()`
* 나눠떨어지지 않으면 올림 처리 필요 (ex. 95%인데 4%씩이면 1.25일 → 2일)
* 배포를 하루에 한번만 가능 구현 가능하다는 것의 의미

### 해결 방법

`first`가 "현재 배포 그룹의 기준일"이라는 걸 이해하고 나니 풀림. `first`보다 먼저/같이 끝나면 같은 그룹, 늦게 끝나면 `first` 갱신하고 새 그룹.

### 시간복잡도 / 공간복잡도

- 시간복잡도: O(n)
- 공간복잡도: O(n)

### 새롭게 알게 된 점

"하루에 한 번만 배포"는 별도 시간 체크가 아니라, 기준 완성일(first)보다 먼저 끝나는 걸 전부 같은 그룹으로 묶는 방식으로 구현됨.

---

## 문제 3

### 문제 정보

- 문제명: 전화번호 목록
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/42577
- 사용한 알고리즘 / 자료구조: 해시 (set)

### 처음 접근 방식

접두어가 있을 때 false, 없으면 true. 문자열 안에 존재하는지를 반복문으로 돌리는 것만 생각했는데, 이 경우는 접두어인지 판단하기 어려움. `"119" in {"119", "674223", "9552"}`로 푸는 줄 알았는데 아니었음.

### 풀이 방법

```python
def solution(phone_book):
    h = set(phone_book)

    for number in phone_book:
        for i in range(1, len(number)):
            prefix = number[:i]
            if prefix in h:
                return False

    return True
```

각 number를 순서대로 슬라이스(number[:i])해서 prefix 만들고, 그 prefix가 h(전체 번호 집합) 안에 정확히 존재하면 접두어 관계 발견 → false.

### 어려웠던 지점

* 보통 배열의 경우 인덱스(정수)를 통해서만 접근이 가능함
* string을 기반으로 정보를 기록하거나 관리할 때 dict/set이 용이함
* prefix in h: 이걸 리스트로 사용할 경우 처음부터 끝까지 다 순회해야함, 반면 집합은 O(1)
* set 썼는데 이게 해시를 쓴 게 맞는지 헷갈렸음 (해시함수를 직접 만든 게 아니라서)

### 해결 방법

set은 내부적으로 해시 테이블로 구현되어 있어서, set을 쓴 것 자체가 해시를 쓴 것과 같음. `in`도 문자열-문자열이면 부분 포함 여부, 문자열-set이면 정확히 원소로 존재하는지라 동작이 다르다는 걸 확인.

### 시간복잡도 / 공간복잡도

- 시간복잡도: O(n × m)
- 공간복잡도: O(n)

### 새롭게 알게 된 점

set은 빈 것 만들 때 `{}`가 아니라 `set()`으로 써야 함 (`{}`는 dict로 인식됨). "해시 유형" 문제는 해시 함수를 직접 구현하는 게 아니라 dict/set으로 존재 여부를 빠르게 확인하는 문제라는 것.

---

# 다음 주 목표

- 목표 문제 수: 3
- 집중할 유형: 해시(다양한 활용 문제), 큐(우선순위 큐 포함), 정렬