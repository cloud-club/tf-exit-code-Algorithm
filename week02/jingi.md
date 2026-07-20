> [!NOTE]
> 다음은 오답노트 예시 템플릿입니다. 자유롭게 수정해서 사용해주세요.
> 이 파일을 복사해서 `week0N/{영문이름}.md`로 저장한 뒤 작성하시면 됩니다.

# N주차 문제 풀이 인증

## 기본 정보

- 이름: 홍진기
- 목표 문제 수: 3
- 실제 풀이 문제 수: 2

## 푼 문제 목록

| 번호 | 문제 이름 | 난이도 | 링크 | 풀이 여부 | 소요 시간 |
| --- | --- | --- | --- | --- | --- |
| 1 | 양궁대회 | level 2 | [프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/92342) | X | 1시간 10분 |
| 2 | 순위 검색 | level 2 | [프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/72412) | X | 1시간 |

---

# 오답노트

> 오답노트는 AI 답변이나 블로그 풀이를 그대로 복사하지 않고, 본인의 언어로 직접 작성해주세요.
> 문제별로 아래 양식을 복사해서 필요한 만큼 반복 작성하시면 됩니다.

## 문제 1

### 문제 정보

- 문제명: 양궁대회
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/92342
- 사용한 알고리즘 / 자료구조: 백트래킹

### 처음 접근 방식

확신은 없었지만 문제의 요구사항, 입력값의 범위를 통해 백트래킹으로 문제를 풀 수 있을 것 같다는 생각을 했다.

하지만 구체적으로 어떤 분기를 기준으로 백트래킹을 하는지 감이 오지 않았다. 과정을 생각하는 데에 많은 시간을 쏟아서 구현 과정에서 코드를 수정하다가 제한 시간 내에 문제를 풀지 못했다.

### 풀이 방법

문제에서 결과적으로 판단해야 하는 것은 **0점부터 10점 사이의 점수 중, 어떤 점수를 라이언이 가져가고, 어떤 점수를 어피치가 가져가야 하는지 결정**하는 것이다.

따라서 간략하게 봤을 때, 백트래킹을 통해 라이언이 이기는 경우를 조건에 맞게 갱신해나간다.

백트래킹을 통해 0점 ~ 10점을 순회하면서 화살의 소비 상태를 추적하여 각 점수를 누가 가져갈지 판단한다.

이때, 라이언이 무조건 높은 순서대로 점수를 가져가는 것이 정답이 아니다. 각 점수마다 필요한 화살의 개수가 다르기도 하고, 가장 낮은 점수를 더 많이 맞힌 경우를 반환해야 하기 때문에 백트래킹으로 이 경우의 수를 모두 구해야한다.

가장 점수 차이가 큰 과녁표를 반환해야 하고, 점수 차이가 같은 경우 낮은 점수를 더 많이 맞힌 경우를 반환해야 하기 때문에 **점수 차이 값**을 지속적으로 갱신하고, 낮은 점수를 더 많이 맞힌 과녁을 반환하는 메소드를 선언하여 정답을 구한다.

```java
class Solution {
    
    static int[] result = new int[11];
    static boolean hasAnswer = false;
    static int diff = 0;
    
    public int[] solution(int n, int[] info) {
        int[] answer = {};
        int[] ryanInfo = new int[11];
        
        // 백트래킹
        dfs(0, n, ryanInfo, info);
        
        for(int i = 0; i < 11; i++){
            if (result[i] != 0){
                hasAnswer = true;
                break;
            }    
        }
        
        return diff > 0 ? result : new int[]{-1};
    }
    
    // index를 순회, 화살 개수 n 추적
    static void dfs(int index, int remaining, int[] ryanInfo, int[] info){
        // 모든 화살을 다 쐈을 경우
        if (index == 10){
            ryanInfo[10] = remaining;    
            // 우승자 결정
            int curDiff = compareScore(ryanInfo, info);
            if (curDiff <= 0) return;
            
            if(diff < curDiff){
                diff = curDiff;
                result = ryanInfo.clone();
            }
            else if(diff == curDiff){
                if (better(result, ryanInfo)){
                    result = ryanInfo.clone();   
                }
            }
        }
        
        else{
            // 점수를 누가 가져갈지 판단하기
            int requiredArrows = info[index]+1;
            // 라이언이 가져가는 경우 - (화살을 어피치보다 한 개 더 씀)
            if (remaining >= requiredArrows) {
                ryanInfo[index] = requiredArrows;
                dfs(index + 1, remaining - requiredArrows, ryanInfo, info);
                ryanInfo[index] = 0;
            }
            // 어피치가 가져가는 경우 - (라이언의 화살이 부족하거나 화살을 쓰지 않는 경우)
            dfs(index + 1, remaining, ryanInfo, info);
        }

    }
    
    // 점수 계산 -> 우승자 결정
    static int compareScore(int[] ryanInfo, int[] info){
        
        int ryanScore = 0;
        int appeachScore = 0;
        
        // 각 과녁 배열 비교
        for(int i = 0 ; i < 11; i++){
            if(ryanInfo[i] > info[i]){
                ryanScore += 10 - i;
            }
            else if (ryanInfo[i] <= info[i] && info[i] != 0){
                appeachScore += 10 - i;
            }
        }
        return ryanScore - appeachScore; 
    }
    
    static boolean better(int[] curRyanInfo, int[] nextRyanInfo){
        
        for(int i = 10; i >=0 ; i--){
            if(curRyanInfo[i] != nextRyanInfo[i]){
                return nextRyanInfo[i] > curRyanInfo[i];
            }
        }
        
        return false;
    }
    
}
```

### 어려웠던 지점

**백트래킹에서 분기처리에 대한 어려움이 있었다.**

라이언이 현재 점수를 얻을 수 있음에도 점수를 내주는 것이 더 유리한 경우가 존재했다.

따라서 아래의 경우를 모두 탐색할 수 있도록 구현해야 했다.

```
화살이 충분한 경우
 ─ 해당 점수를 획득
 ─ 해당 점수를 포기

화살이 부족한 경우
 ─ 해당 점수를 포기
```

하지만 나는 처음에 `if` 조건을 "어피치가 점수를 가져가는 경우"와 "라이언이 점수를 가져가는 경우"로 잘못 잡아서 이를 수정하는 데 많은 시간이 소요됐다.

즉, 어피치가 점수를 가져가는 경우에는 "라이언이 화살을 맞힐 수 있지만 그러지 않은 경우"와 "라이언이 화살을 애초에 맞히지 못하는 경우"가 있어 위 조건을 기준으로는 모든 분기를 처리할 수 없었다.

### 해결 방법

테스트케이스를 실행해본 뒤, 정답이 일치하지 않아 직접 코드의 실행 흐름을 파악하는 과정에서 문제를 파악하고 해결할 수 있었다.

### 시간복잡도 / 공간복잡도

- 시간복잡도:
- 공간복잡도:

### 새롭게 알게 된 점

문제의 요구사항이 많았고, 어떤 기준으로 백트래킹을 시도할지를 문제에서 직관적으로 찾기가 힘들었던 것 같다.

심지어 시간의 압박을 받아 구현하면서 구체적인 로직을 작성해나가다 보니, 코드의 실행 흐름을 파악하고 중간중간 수정하는 과정에서 코드의 이해가 어려웠고, 코드가 복잡해졌다. 

문제의 요구사항이 많아도 문제를 정확히 이해하고, 테스트케이스 및 여러 시나리오를 예상한 뒤 적절한 알고리즘을 선택한 다음에 구현을 옮기는 과정이 더 필요하다고 느낀 문제였다.

앞으로 많은 문제를 풀어보면서 이러한 역량을 키워나가야겠다.

---

## 문제 2

### 문제 정보

- 문제명: 순위 검색
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/72412
- 사용한 알고리즘 / 자료구조: 맵, 이분탐색, DFS

### 처음 접근 방식

직관적으로 여러 개의 쿼리 중 각 쿼리를 실행할 때마다 지원자 정보를 전부 검사하는 것은 안된다고 생각했다.

지원자의 정보와 쿼리값이 각각 5만개와 10만개로, 너무 컸기 때문이다. 따라서 어떻게든 다른 방식을 찾으려고 했다.

지원자 정보를 하나를 대상으로 쿼리를 모두 탐색하고, 해당되는 쿼리가 있으면 값을 1 증가해나가는 방식도 고민했지만, 결국 이것도 순서만 다를 뿐 처음 언급한 과정과 복잡도는 같았다.

결국 1시간 동안 구현할 수 있는 과정을 떠올리지 못했고, 힌트를 보면서 문제를 풀어나갔다.

### 풀이 방법

가장 중요한 것은 문제는 **탐색하는 로직을 최적화하는 것**이다. 하지만 데이터를 탐색하는 로직이 아닌, 데이터를 빠르게 탐색할 수 있도록 전처리하는 것이 핵심이다.

생각해보면 쿼리에서 "특정 점수 이상의 값"은 이분탐색을 통해 구현할 수 있고, '문제는 나머지 조건을 어떻게 처리하는가'이다.

점수 이외에 문제에서 나올 수 있는 조건의 조합은 **언어 3개, 직무 2개, 경력 2개, 선호 음식 2개, 여기에 각 항목이 '-'로 대체될 수 있으므로 (3+1)x(2+1)x(2+1)x(2+1) : 108개**이다.

이 모든 경우의 수를 각각 Map에 저장하면 빠른 탐색이 가능하다. (Ex. java, be, junior, pizza -> "javabejuniorpizza")

그리고 지원자의 정보를 Map에 저장할 때, 추후 query에서 정확한 조건이 아닌 "-" 문자를 통해서도 탐색될 수 있으므로 **지원자의 정보에 "-"가 중복으로 들어갈 수 있는 16개의 Key에도 데이터를 추가하는 작업이 필요하다.**

정리하자면 다음과 같다.

- 탐색은 해시를 사용 `Map<String, List<Integer>>`
- 지원자의 정보를 저장
- DFS를 활용하여 "-"으로 탐색될 수 있는 경우의 수도 모두 저장
- 특정 점수 이상의 값은 이분탐색으로 조회
- query의 결과를 Map에서 조회하여 순차적으로 answer 배열에 저장

```java
import java.util.*;

class Solution {
    
    static Map<String, ArrayList<Integer>> map = new HashMap<>();
    
    public int[] solution(String[] info, String[] query) {
        int[] answer = new int[query.length];
        
        StringTokenizer st;
        
        // 지원자 정보 파싱 후 맵에 저장
        for(int i = 0 ; i < info.length; i++){
            st = new StringTokenizer(info[i]);
            String lang = st.nextToken();
            String role = st.nextToken();
            String career = st.nextToken();
            String food = st.nextToken();
            Integer score = Integer.parseInt(st.nextToken());
            
            String key = lang + role + career + food;
            
            // query로 탐색될 수 있는 모든 경우의 수 저장
            dfs(0, new String[] {lang, role, career, food}, score);
            
        }
        
        // value 값들 모두 정렬
        for(Map.Entry<String, ArrayList<Integer>> entry : map.entrySet()){
            ArrayList<Integer> sorted = entry.getValue();
            sorted.sort(Comparator.naturalOrder());
            map.put(entry.getKey(), sorted);
        }
        
        // 쿼리 정보 파싱 후 값 조회 후 answer에 차례대로 저장(이분탐색)
        for(int i = 0 ; i < query.length; i++){
            String replaced = query[i].replace(" and ", " ");
            st = new StringTokenizer(replaced);
            String lang = st.nextToken();
            String role = st.nextToken();
            String career = st.nextToken();
            String food = st.nextToken();
            
            Integer lowerBound = Integer.parseInt(st.nextToken());
            
            String findKey = lang + role + career + food;
            
            ArrayList<Integer> infoList = map.get(findKey);
            
            // query로 조회한 값이 없을 경우 0 처리
            if (infoList == null){
                answer[i] = 0;
                continue;
            }
                
            // 이분탐색으로 lowerBound 탐색
            int lowerBoundIdx = binSearch(infoList, lowerBound);
            int count = 0;
            
            answer[i] = infoList.size() - lowerBoundIdx;
        }
        
        return answer;
    }
    
    // 경우의 수 모두 저장
    static void dfs(int depth, String[] info, int score){
        
        if (depth == 4){
            String key = info[0] + info[1] + info[2] + info[3];
            
            // 있으면 리스트에 score 추가, 없으면 새로 k, v 추가
            if (map.containsKey(key)){
                ArrayList<Integer> list = map.get(key);
                list.add(score);
                map.put(key, list);
            } else {
                ArrayList<Integer> list = new ArrayList<>();
                list.add(score);
                map.put(key, list);
            }
         
        }
        
        else{
            String original = info[depth];
            dfs(depth + 1, info, score);
            info[depth] = "-";
            dfs(depth + 1, info, score);
            info[depth] = original;
        }
        
    }
    
    // 이분탐색
    static int binSearch(ArrayList<Integer> info, int lowerBound){
        
        int left = 0;
        int right = info.size();
        
        while(left < right){
            
            int middle = (left + right) / 2;
            
            if (info.get(middle) >= lowerBound){
                right = middle;
            } else {
                left = middle + 1;
            }
            
        }
        return left;
    }
            
}
```

### 어려웠던 지점

"-" 탐색 조건을 처리하는 것이 구현 과정에서 어려웠다.

처음에는 "-" 경우까지 모두 Key로 저장하는 방식을 생각하지 못했다.

그래서 "-"가 나올 경우, 여러 Key 값을 탐색해서 계산을 해야되나 싶었지만, 여러 조건을 합친 Key 값에서 "-"를 적용하기가 어려웠다.

### 해결 방법

결국 지원자 정보를 저장하는 시점에 "-"로 조회될 수 있는 모든 경우의 수도 Key로 만들어 저장했다.

예를 들어 java backend junior pizza라는 지원자는 "-"를 포함하는 java backend junior -, java - junior pizza, - backend - pizza 등의 조건으로도 조회될 수 있다.

따라서 한 지원자 정보에 "-"가 포함될 수 있는 모든 경우를 DFS로 탐색했고, 이를 통해 지원자 한 명당 2^4 = 16개의 Key를 생성하고, 각 Key의 점수 리스트에 해당 지원자의 점수를 저장하여 앞서 언급했던 풀이 방법이 되었다.

### 시간복잡도 / 공간복잡도

- 시간복잡도: O(N log N + Q log N) (N은 지원자 정보 개수, 각 리스트마다 정렬 / Q는 쿼리 개수, 쿼리마다 이분탐색 실행)
- 공간복잡도: O(N + Q) (DFS는 깊이가 4로 고정)

### 새롭게 알게 된 점

빠르게 탐색하기 위해서는 단순히 탐색 알고리즘만 고려하는 것이 아닌, 탐색 대상이 되는 데이터를 어떻게 구성할지도 중요하다는 것을 알게 되었다.

앞으로는 이와 같이 탐색 문제가 주어졌을 때, 단순히 탐색 알고리즘 뿐만 아니라 문제에서 요구하느 값을 효과적으로 찾으려면 데이터를 어떻게 구성해야 하는지도 고민해봐야겠다.