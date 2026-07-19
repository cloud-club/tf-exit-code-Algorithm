

# 1주차 문제 풀이 인증

## 기본 정보

- 이름: 주정윤
- 목표 문제 수: 3
- 실제 풀이 문제 수: 3

## 푼 문제 목록

| 번호 | 문제 이름 | 난이도 | 링크 | 풀이 여부 |
| --- | --- | --- | --- | --- |
| 1 | 신규 아이디 추천 | Lv.1 | https://school.programmers.co.kr/learn/courses/30/lessons/72410 |  |
| 2 | 체육복 | Lv.1 | https://school.programmers.co.kr/learn/courses/30/lessons/42862 |  |
| 3 | 문자열 압축 | Lv.2 | https://school.programmers.co.kr/learn/courses/30/lessons/60057 |  |

---

## 문제 1

### 문제 정보

- 문제명: 신규 아이디 추처
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/72410
- 사용한 알고리즘 / 자료구조: 문자열, StringBuilder, 구현

### 처음 접근 방식

알고리즘 풀이가 너무 오랜만이라 허둥댔는데 단순 구현 문제임을 알고 접근했다. 처음에는 정규표현식을 사용할 수도 있겠다고 생각했지만 문자열을 직접 순회하면서 조건에 맞지 않는 문자를 제거하고 필요한 경우 새로운 문자열을 만들어가는 방식이 더 이해하기 쉽다고 판단했다. 따라서 각 단계를 그대로 코드로 옮기는 방식으로 구현하였다.

### 풀이 방법

문제에서 제시한 7단계를 그대로 구현하였다.

1. 문자열의 모든 대문자를 소문자로 변환한다.
2. 허용되지 않는 문자를 하나씩 검사하여 제거한다.
3. 연속된 마침표(`..`)가 나오면 하나의 마침표(`.`)만 남도록 처리한다.
4. 문자열의 처음과 끝에 있는 마침표를 제거한다.
5. 문자열이 비어 있다면 `"a"`를 대입한다.
6. 문자열 길이가 16자 이상이면 앞의 15글자만 남기고, 마지막 문자가 마침표라면 제거한다.
7. 문자열 길이가 2자 이하라면 마지막 문자를 반복해서 붙여 길이를 3으로 만든다.

문자열을 계속 수정해야 했기 때문에 `StringBuilder`를 사용하여 새로운 문자열을 만들어가며 처리하였다.

```java
class Solution {
    public String solution(String new_id) {
        
        // 1단계 대문자 -> 소문자 치환
        new_id = new_id.toLowerCase();

        // 2단계 특정 문자 제외한 모든 문자를 제거
        StringBuilder sb = new StringBuilder();
        for(char c : new_id.toCharArray()){
            if(c >= 'a' && c <= 'z'|| '0' <= c && c <= '9' || c == '-' || c == '_' || c == '.'){
                sb.append(c);
            }
        }
        new_id = sb.toString();

        // 3단계 마침표 2번 이상 연속된 부분을 하나의 마침표로 치환
        sb = new StringBuilder();
        boolean flag = false;
        for(char c : new_id.toCharArray()){
            if(c == '.'){
                if(!flag){
                    sb.append(c);
                    flag = true;
                }
            }
            else {
                sb.append(c);
                flag= false;
            }
        }

        new_id = sb.toString();

        // 4단계 마침표가 처음이나 끝에 위치한다면 제거
        sb = new StringBuilder();
        for(int i=0; i<new_id.length(); i++){
            if((i==0 || i == new_id.length() - 1) && new_id.charAt(i) == '.') continue;
            sb.append(new_id.charAt(i));
        }
        new_id = sb.toString();

        // 5단계 빈 문자열이라면 "a"를 대입
        if(new_id.length() < 1) new_id = "a";

        // 6단계 길이가 16자 이상이면 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거
        sb = new StringBuilder();
        if(new_id.length() >= 16){
            for(int i = 0; i<15; i++){
                sb.append(new_id.charAt(i));
            }

            new_id = sb.toString();

            // 맨 끝에 위치한 마침표 문자가 위치한다면 제거
            if(new_id.charAt(14) == '.') {
                new_id = new_id.substring(0, 14);
            }
        }

        // 7단계 길이가 2자 이하라면, 마지막 문자를 길이가 3이 될 때까지 반복해서 끝에 붙임
        sb = new StringBuilder();
        if(new_id.length() <= 2){
            char c = new_id.charAt(new_id.length()-1);
            sb.append(new_id);

            while(true){
                if(sb.length() >= 3) break;
                sb.append(c);
            }

            new_id = sb.toString();
        }

        return new_id;
    }
}
```

### 어려웠던 지점

가장 신경 썼던 부분은 문자열의 길이가 계속 변하는 점이었다.

특히 6단계에서 문자열을 15글자로 자른 뒤 마지막 문자가 마침표인지 다시 확인해야 하는 부분과 7단계에서 마지막 문자를 반복해서 붙여 길이를 맞추는 부분에서 인덱스를 잘못 사용하면 예외가 발생할 수 있어 주의하였다.

또한 문자열을 수정한 뒤에는 항상 변경된 문자열을 기준으로 다음 단계를 수행해야 한다는 점도 놓치지 않으려고 했다.

### 해결 방법

각 단계를 독립적으로 구현하고, 단계가 끝날 때마다 `new_id`를 갱신하도록 작성하였다.

문제에서 제시한 처리 순서를 그대로 따라가면서 구현하니 이전 단계의 결과가 다음 단계의 입력이 되는 구조를 자연스럽게 만들 수 있었다.

구현을 마친 뒤에는 예시 테스트를 직접 따라가며 각 단계에서 문자열이 올바르게 변경되는지 확인하여 실수를 줄였다.

### 시간복잡도 / 공간복잡도

- **시간복잡도:** O(N)
    - 문자열을 여러 번 순회하지만 각 단계가 최대 한 번씩만 문자열을 검사하므로 전체적으로 선형 시간에 해결할 수 있다.
- **공간복잡도:** O(N)
    - 문자열을 수정하기 위해 `StringBuilder`와 새로운 문자열을 생성하였다.

### 새롭게 알게 된 점

이번 문제를 통해 구현 문제에서는 복잡한 알고리즘보다 문제에서 요구하는 조건을 정확하게 구현하는 것이 더 중요하다는 것을 느꼈다.

또한 문자열은 변하지 않기 때문에 반복해서 수정해야 하는 경우에는 `StringBuilder`를 사용하는 것이 효율적이라는 점을 다시 확인할 수 있었다.

추가로 문자열을 자르거나 문자를 참조할 때는 항상 길이를 먼저 확인하여 인덱스 오류가 발생하지 않도록 습관을 들여야겠다고 생각했다.

---

## 문제 2

### 문제 정보

- 문제명: 체육복
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/42862
- 사용한 알고리즘 / 자료구조: 그리디, 배열, 정렬

### 처음 접근 방식

처음에는 체육복을 빌려줄 수 있는 학생이 앞번호 또는 뒷번호 학생에게만 빌려줄 수 있다는 점에 주목했다.

먼저 체육복을 도난당하지 않은 학생 수를 기본값으로 계산한 뒤, **여벌 체육복을 가져왔지만 도난도 당한 학생을 먼저 처리해야 한다**고 생각했다. 이 학생은 자신의 체육복을 직접 사용해야 하므로 다른 학생에게 빌려줄 수 없기 때문이다.

그 이후 남은 학생들끼리 앞번호 또는 뒷번호 학생에게 체육복을 빌려주는 방식으로 구현하였다.

### 풀이 방법

```java
import java.util.*;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        
        Arrays.sort(lost);
        Arrays.sort(reserve);
        
        answer = n - lost.length; //도난당하지 않은 학생 수
        
        //여벌을 챙겨온 사람이 도난을 당한 케이스 > 본인이 본인에게 빌려줘야됨
        for(int i=0; i<lost.length; i++){
            for(int j=0; j<reserve.length; j++){
                if(reserve[j] == lost[i]){
                    answer++;
                    reserve[j] = -1;
                    lost[i] = -1;
                    break;
                } 
            }
        }
        
        //여벌의 체육복을 앞번호 사람에게 주는 케이스
        for(int i=0; i<lost.length; i++){
            for(int j=0; j<reserve.length; j++){
                if(reserve[j] == lost[i]-1 || reserve[j] == lost[i]+1){
                    answer++;
                    lost[i] = -1;
                    reserve[j] = -1;
                    break;
                }
            }
        }
        
        
        return answer;
    }
}
```

### 어려웠던 지점

가장 어려웠던 부분은 **여벌 체육복이 있으면서 동시에 도난당한 학생을 먼저 처리해야 한다는 점**이었다.

이 과정을 먼저 처리하지 않으면 실제로는 다른 학생에게 빌려줄 수 없는 학생이 여벌 체육복을 빌려주는 것으로 처리될 수 있어 결과가 달라질 수 있다.

또한 한 학생의 여벌 체육복은 한 번만 사용할 수 있으므로 이미 사용한 학생을 다시 사용하지 않도록 관리하는 부분도 신경 써야 했다.

### 해결 방법

먼저 자기 자신의 체육복을 사용하는 학생을 모두 처리한 뒤, 남은 학생들끼리만 대여하도록 구현하였다.

체육복을 사용한 학생은 `-1`로 변경하여 이후 반복문에서 다시 선택되지 않도록 하였다. 이렇게 처리하니 한 학생이 여러 번 체육복을 빌려주는 경우를 방지할 수 있었다.

### 시간복잡도 / 공간복잡도

- **시간복잡도:** **O(L × R)**
    - `lost`와 `reserve`를 각각 두 번 중첩 반복문으로 탐색하였다.
    - (`L`: lost 배열의 길이, `R`: reserve 배열의 길이)
    - 정렬 비용까지 포함하면 `O(L log L + R log R + L × R)`이다.
- **공간복잡도:** **O(1)**
    - 추가적인 자료구조를 사용하지 않고 기존 배열만 수정하여 사용하였다.

### 새롭게 알게 된 점

이번 문제를 통해 **그리디 문제에서는 처리 순서가 매우 중요하다**는 것을 느꼈다.

특히 여벌 체육복을 가져왔지만 도난도 당한 학생을 먼저 처리하지 않으면 올바른 결과를 얻을 수 없다는 점을 배웠다.

또한 이미 사용한 데이터를 별도의 배열을 만들지 않고 특정 값(`-1`)으로 표시하여 중복 사용을 막는 방법도 구현 문제에서 유용하게 사용할 수 있다는 것을 알게 되었다.

---

## 문제 3

### 문제 정보

- 문제명: 문자열 압축
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/60057?language=java
- 사용한 알고리즘 / 자료구조: 구현, 문자열, StringBuilder

### 처음 접근 방식

처음에는 문자열을 어떤 기준으로 압축해야 하는지 고민했다. 문제를 읽어보니 압축 단위가 고정되어 있는 것이 아니라 **1글자부터 문자열 길이의 절반까지 모든 단위를 직접 확인해야 한다**는 점이 핵심이라고 생각했다.

각 단위마다 압축 결과가 달라질 수 있으므로, 모든 경우를 직접 구현해서 가장 짧은 길이를 찾는 방식으로 접근하였다.

### 풀이 방법

```java
class Solution {
    public int solution(String s) {

        if (s.length() == 1) return 1;

        int answer = s.length();

        for (int size = 1; size <= s.length() / 2; size++) {

            StringBuilder sb = new StringBuilder();

            String prev = s.substring(0, size);
            int count = 1;

            int i;

            for (i = size; i + size <= s.length(); i += size) {

                String cur = s.substring(i, i + size);

                if (prev.equals(cur)) {
                    count++;
                } else {

                    if (count > 1)
                        sb.append(count);

                    sb.append(prev);

                    prev = cur;
                    count = 1;
                }
            }

            if (count > 1)
                sb.append(count);

            sb.append(prev);

            if (i < s.length()) {
                sb.append(s.substring(i));
            }

            answer = Math.min(answer, sb.length());
        }

        return answer;
    }
}
```

### 어려웠던 지점

가장 어려웠던 부분은 **문자열의 끝부분 처리**였다.

문자열 길이가 압축 단위로 나누어떨어지지 않는 경우 마지막에 남는 문자열은 압축하지 않고 그대로 붙여야 한다. 또한 반복이 끝난 마지막 문자열은 반복문 안에서 처리되지 않기 때문에 반복문이 종료된 뒤 별도로 추가해야 했다.

처음에는 반복문 안에서만 처리하려고 했지만 마지막 문자열이 누락되는 경우가 발생할 수 있어 반복문 밖에서 마지막 데이터를 처리하도록 구현하였다.

### 해결 방법

문자열을 일정한 단위로 잘라 이전 문자열과 현재 문자열을 비교하는 방식을 사용하였다.

반복되는 문자열은 개수를 세다가 다른 문자열이 나오면 그동안의 결과를 `StringBuilder`에 추가하였다. 마지막 문자열은 반복문이 끝난 후 한 번 더 처리하여 누락되지 않도록 구현하였다.

또한 압축 단위를 1부터 문자열 길이의 절반까지 모두 확인하면서 가장 짧은 압축 길이를 계속 갱신하였다.

### 시간복잡도 / 공간복잡도

- **시간복잡도:** **O(N²)**
    - 압축 단위를 최대 `N/2`개 확인하고, 각 단위마다 문자열 전체를 순회한다.
- **공간복잡도:** **O(N)**
    - 압축된 문자열을 저장하기 위해 `StringBuilder`를 사용하였다.

### 새롭게 알게 된 점

이번 문제를 통해 구현 문제에서는 **모든 경우를 빠짐없이 확인하는 완전 탐색 방식**도 충분히 사용할 수 있다는 것을 배웠다.

또한 문자열을 일정한 크기로 잘라 비교하는 문제에서는 `substring()`을 이용하면 구현이 훨씬 간단해진다는 점을 알게 되었다.

특히 반복되는 문자열을 처리할 때는 **이전 문자열(prev), 현재 문자열(cur), 반복 횟수(count)** 세 가지 정보를 관리하면 대부분의 문자열 압축 문제를 쉽게 구현할 수 있다는 점을 배웠다.

다음에 비슷한 문제가 나온다면 문자열을 직접 수정하기보다 `StringBuilder`를 이용해 새로운 문자열을 만들어 가는 방식으로 구현하면 코드도 더 깔끔하고 실수도 줄일 수 있을 것 같다.

---

# 다음 주 목표

- 목표 문제 수: 3
- 집중할 유형: Stack / Queue / Hash / 정렬