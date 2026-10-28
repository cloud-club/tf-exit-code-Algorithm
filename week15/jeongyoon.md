# 15주차 문제 풀이 인증

## 기본 정보

- 이름: 주정윤
- 목표 문제 수: 3
- 실제 풀이 문제 수: 3

---

# 문제 요약

| 번호 | 문제 이름 | 난이도 | 링크 |
| --- | --- | --- | --- |
| 1 | 귤 고르기 | Lv.2 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/138476) |
| 2 | 신고 결과 받기 | Lv.1 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/92334) |
| 3 | [3차] 압축 | Lv.2 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/17684) |

---

# 오답노트

## 문제 1

- **문제명:** 귤 고르기
- **문제 링크:** [https://school.programmers.co.kr/learn/courses/30/lessons/138476](https://school.programmers.co.kr/learn/courses/30/lessons/138476)
- **알고리즘 / 자료구조:** 해시, 정렬, 그리디

### 접근 및 시행착오

귤을 k개 선택할 때 크기가 서로 다른 종류의 수를 최소화해야 한다. 따라서 같은 크기의 귤이 많이 있는 종류부터 선택하면 된다고 생각했다.

먼저 `HashMap`에 귤의 크기별 개수를 저장하고, 각 크기와 개수를 배열에 담아 개수가 많은 순서대로 정렬하는 방식으로 접근했다.

### 최종 풀이

1. `HashMap`을 사용해 귤의 크기별 개수를 센다.
2. 각 크기와 개수를 `ArrayList<int[]>`에 저장한다.
3. 귤의 개수가 많은 순서대로 리스트를 정렬한다.
4. 개수가 많은 종류부터 선택하면서 필요한 귤의 수인 k에서 선택한 개수를 뺀다.
5. k가 0 이하가 되면 선택한 종류의 개수를 반환한다.

### 내가 푼 코드

```java
import java.util.*;

class Solution {
    public int solution(int k, int[] tangerine) {
        int answer = 0;

        ArrayList<int[]> list = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();

        for(int t : tangerine){
            if(map.containsKey(t)){
                map.put(t, map.get(t) + 1);
            } else {
                map.put(t, 1);
            }
        }

        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
            int a = entry.getKey();
            int b = entry.getValue();

            list.add(new int[]{a, b});
        }

        list.sort((a, b) -> b[1] - a[1]);

        for(int[] t : list){
            k -= t[1];
            answer++;

            if(k <= 0){
                return answer;
            }
        }

        return answer;
    }
}
```

### 복잡도

- 시간복잡도: O(N + M log M)
- 공간복잡도: O(M)

※ N은 전체 귤의 개수, M은 귤의 종류 수이다.

### 핵심 포인트

종류의 수를 최소화하는 문제에서는 각 종류의 개수를 구한 뒤, 개수가 많은 종류부터 선택하는 그리디 전략을 사용할 수 있다.

---

## 문제 2

- **문제명:** 신고 결과 받기
- **문제 링크:** [https://school.programmers.co.kr/learn/courses/30/lessons/92334](https://school.programmers.co.kr/learn/courses/30/lessons/92334)
- **알고리즘 / 자료구조:** 해시, 집합

### 접근 및 시행착오

한 유저가 같은 사람을 여러 번 신고해도 신고 횟수는 한 번만 인정된다는 조건이 중요하다.

신고당한 횟수를 저장하고, 각 유저가 신고한 대상도 따로 저장하면 정지된 유저를 신고한 사람에게 메일을 몇 번 보내야 하는지 구할 수 있다. 중복 신고를 처리하기 위해 `HashSet`을 사용했다.

### 최종 풀이

1. 신고당한 횟수를 저장할 `reportSum`과 각 유저가 신고한 대상을 저장할 `reportList`를 초기화한다.
2. 신고 기록을 순회하면서 `"신고자 신고대상"`을 분리한다.
3. 같은 신고자가 같은 대상을 중복 신고한 경우는 제외한다.
4. 새로운 신고라면 신고 대상의 신고 횟수를 증가시키고, 신고자의 신고 목록에 대상을 추가한다.
5. 신고당한 횟수가 k 이상인 유저를 확인한다.
6. 각 유저가 신고한 대상 중 정지된 유저의 수를 세어 결과 배열에 저장한다.

### 내가 푼 코드

```java
import java.util.*;

class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        HashMap<String, Integer> reportSum = new HashMap<>();
        HashMap<String, ArrayList<String>> reportList = new HashMap<>();
        HashSet<String> reportSet = new HashSet<>();

        for(String i : id_list){
            reportSum.put(i, 0);
            reportList.put(i, new ArrayList<>());
        }

        for(String i : report){
            if(!reportSet.add(i)){
                continue;
            }

            String[] report_l = i.split(" ");
            String reporter = report_l[0];
            String problem = report_l[1];

            reportSum.put(problem, reportSum.get(problem) + 1);
            reportList.get(reporter).add(problem);
        }

        int[] answer = new int[id_list.length];

        for(int i = 0; i < id_list.length; i++){
            String user = id_list[i];

            for(String reported : reportList.get(user)){
                if(reportSum.get(reported) >= k){
                    answer[i]++;
                }
            }
        }

        return answer;
    }
}
```

### 복잡도

- 시간복잡도: O(R + U × L)
- 공간복잡도: O(R + U + L)

※ R은 신고 기록의 개수, U는 유저 수, L은 유저 한 명이 신고한 대상의 최대 개수이다.

### 핵심 포인트

중복을 허용하지 않는 신고 기록은 `HashSet`으로 처리할 수 있다. 신고 횟수와 신고자별 목록을 각각 관리하면 정지 대상과 결과 메일 수를 효율적으로 계산할 수 있다.

---

## 문제 3

- **문제명:** [3차] 압축
- **문제 링크:** [https://school.programmers.co.kr/learn/courses/30/lessons/17684](https://school.programmers.co.kr/learn/courses/30/lessons/17684)
- **알고리즘 / 자료구조:** 문자열, 해시, 구현, LZW 압축

### 접근 및 시행착오

LZW 압축은 사전에서 현재 입력과 일치하는 가장 긴 문자열을 찾고, 해당 문자열의 색인 번호를 출력하는 방식이다.

처음에는 알파벳 A부터 Z까지를 사전에 저장한 다음, 현재 문자열에 다음 문자를 계속 붙여 사전에 존재하는지 확인하는 방식으로 접근했다. 사전에 없는 문자열을 발견하면 직전까지 일치했던 문자열의 색인 번호를 출력하고, 새로운 문자열을 사전에 추가하면 된다.

### 최종 풀이

1. `HashMap`에 A부터 Z까지의 알파벳과 색인 번호 1~26을 저장한다.
2. 새로 추가할 사전 색인 번호를 27로 초기화한다.
3. 현재 위치에서 시작하는 문자열 `w`에 다음 문자를 하나씩 붙여 사전에 존재하는지 확인한다.
4. 사전에 존재하면 현재 문자열을 확장하고 다음 문자를 확인한다.
5. 사전에 없는 문자열을 만나면 기존 문자열 `w`의 색인 번호를 결과에 추가하고, 새로운 문자열 `w + c`를 사전에 등록한다.
6. 다음 처리 위치를 갱신하고 입력 문자열을 모두 처리할 때까지 반복한다.
7. 마지막에 남은 문자열의 색인 번호도 결과에 추가한 뒤 `ArrayList<Integer>`를 `int[]`로 변환해 반환한다.

### 내가 푼 코드

```java
import java.util.*;

class Solution {
    public int[] solution(String msg) {
        HashMap<String, Integer> map = new HashMap<>();
        ArrayList<Integer> list = new ArrayList<>();

        for(int i = 0; i < 26; i++){
            map.put(String.valueOf((char)('A' + i)), i + 1);
        }

        int index = 27;
        String[] msg_split = msg.split("");

        int i = 0;

        while(i < msg_split.length){
            String w = "";
            String confirm = "";

            w += msg_split[i];
            int j = i + 1;

            while(j < msg_split.length){
                confirm = w + msg_split[j];

                if(map.containsKey(confirm)){
                    w = confirm;
                    j++;
                } else {
                    list.add(map.get(w));
                    map.put(confirm, index);
                    index++;
                    break;
                }
            }

            i = j;

            if(j == msg_split.length){
                list.add(map.get(w));
            }
        }

        int[] answer = new int[list.size()];

        for(int a = 0; a < list.size(); a++){
            answer[a] = list.get(a);
        }

        return answer;
    }
}
```

### 복잡도

- 시간복잡도: O(N²)
- 공간복잡도: O(N²)

※ N은 입력 문자열의 길이이다. 문자열 이어 붙이기와 사전 저장 공간까지 고려한 보수적인 복잡도이다.

### 핵심 포인트

현재 입력에서 사전에 존재하는 가장 긴 문자열을 찾는 것이 핵심이다. 사전에 없는 문자열을 발견하면 기존 문자열의 색인 번호를 출력하고, 기존 문자열에 다음 문자를 붙인 새로운 문자열을 사전에 등록한다.

---

# 다음 주 목표

- 목표 문제 수: 3
- 집중할 유형: 해시, 그리디, 문자열 구현
