# 1주차 문제 풀이 인증

## 기본 정보

- 이름: 홍진기
- 목표 문제 수: 3
- 실제 풀이 문제 수:

## 푼 문제 목록

| 번호 | 문제 이름 | 난이도 | 링크 | 풀이 여부 | 소요 시간 |
| --- | --- | --- | --- | --- | --- |
| 1 | 이모티콘 할인 행사 | level 2, 정답률 44% | [프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/150368?language=java) | O | 58분 20초 | 
| 2 |  |  |  |  |  |
| 3 |  |  |  |  |  |

---

# 오답노트

> 오답노트는 AI 답변이나 블로그 풀이를 그대로 복사하지 않고, 본인의 언어로 직접 작성해주세요.
> 문제별로 아래 양식을 복사해서 필요한 만큼 반복 작성하시면 됩니다.

## 문제 1

### **문제 정보**

- 문제명: 이모티콘 할인 행사
- 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/150368?language=java
- 사용한 알고리즘 / 자료구조: `완전 탐색`, `구현`

### 풀이 시간

58분 20초

### **처음 접근 방식**

문제를 이해한 후, 어떤 알고리즘을 사용해야 하는지 빠르게 판단할 수 있었다.

결국 **문제의 조건을 충족하기 위해 각 이모티콘의 최적의 할인 비율 선정**

이모티콘 가격도 입력값에 따라 다 달라지기 때문에 모든 경우의 수를 다 따져봐야 된다고 생각했다. → **백트래킹 또는 완전탐색**

모든 이모티콘의 할인 비율을 선택한 뒤에 결과값 비교가 가능하기 때문에 → **완전탐색**

완전탐색을 생각하고 시간 복잡도를 고려했다.

시간 복잡도는 이모티콘의 개수가 최대 7개이고, 적용 가능한 할인 비율은 4개이므로, 이모티콘의 입력값이 최대일 경우 탐색해야 할 경우의 수는 $4^{7}$ 약 16000가지다.

여기에 각 경우의 수마다 사용자 수만큼 반복하고, 사용자 한 명 당 다시 이모티콘 수만큼 순회하므로 실제 총 연산 횟수는 대략 $4^{7} \times 100 \times 7$, 천만 번이다.

통상적으로 1초에 약 1억번 내외의 연산을 처리할 수 있다고 가정하면 충분하다고 판단했다.

### **풀이 방법**

주석으로 어떻게 구현할지 생각하고, 이를 코드로 옮겼다.

```java
class Solution {
    
    public int[] solution(int[][] users, int[] emoticons) {
        
        int[] answer = {};
     
        // 이모티콘들의 할인 비율 -> 결과 결정 : 완전탐색 (할인비율 4, 이모티콘 7)

        return answer;
    }
    
    static void dfs(){ 
        
        // 탐색 끝일 경우 : 할인 가격 모두 적용 시(depth=7) result 검증

            // 사용자 당 구매액 계산
            
            // 기존 결과와 새 결과 비교 후 결과 반환
        
        // 완전탐색 (할인 비율 개수만큼 반복)

    }
}
```

최종 코드는 다음과 같다. 

```java
class Solution {
    
    static double[] rates = {0.1, 0.2, 0.3, 0.4};
    static int[] answer = new int[2];
    
    public int[] solution(int[][] users, int[] emoticons) {
        
        // 각 이모티콘들의 할인 비율
        double[] selectedRates = new double[emoticons.length];
        
        // 이모티콘들의 할인 비율 -> 결과 결정 : 완전탐색 (할인비율 4, 이모티콘 7)
        dfs(0, users, emoticons, selectedRates);

        return answer;
    }
    
    static void dfs(int depth, 
             int[][] users,
             int[] emoticons, 
             double[] selectedRates){ 
        
        // 탐색 끝일 경우 : 할인 가격 모두 적용 시 result 검증
        if (depth == emoticons.length){
            answer = selectResult(answer, users, emoticons, selectedRates);
        }
        else{
            // 완전탐색 (할인 비율 개수만큼 반복)
            for(int i = 0; i < 4; i++){
                selectedRates[depth] = rates[i];
                dfs(depth + 1, users, emoticons, selectedRates);
            }
        }
    }
    
    static int[] selectResult(int[] answer, 
                       int[][] users, 
                       int[] emoticons, 
                       double[] selectedRates){
        
        int[] result = new int[2];
        
        // 사용자 당 구매액 계산
        for (int i = 0 ; i < users.length; i++){

            int minimumRate = users[i][0];
            int priceLimit = users[i][1];
            
            int purchaseTotal = 0;
            
            for(int j = 0; j < emoticons.length; j++){
                // 이모티콘의 할인 비율이 "사용자의 비율" 이상일 경우 > 구매
                if(minimumRate <= (int)(selectedRates[j]*100)){
                    purchaseTotal += (double)emoticons[j] * (1-selectedRates[j]);
                }
            }
            // 이모티콘 플러스 가입 여부 결정
            if (purchaseTotal >= priceLimit){
                result[0]++;
            }
            else{
                result[1] += purchaseTotal;
            }
            
        }
        // 기존 결과와 새 결과 비교 후 결과 반환
        if (result[0] > answer[0]){
            return result;
        } else if (result[0] == answer[0]){
            return result[1] > answer[1] ? result : answer;
        } else {
            return answer;
        }
        
    }
    
}
```

결과값을 비교하는 로직이 생각보다 복잡할 것 같아서 처음 주석으로 계획했을 때와는 다르게 결과 비교 로직을 별도의 메소드로 분리했다.

따라서 **완전탐색 `dfs` → 이모티콘들의 할인 비율 결정 시 → 결과 비교 `selectResult` → 반복**

- **`dfs()`**
    
    완전탐색을 진행하는 메소드이다.
    
    각 이모티콘들(`emoticons[]`)에 대한 할인 경우의 수를 모두 탐색 한 뒤, 결과를 `selectResult()`로 결과를 비교한다.
    
- **`selectResult()`**
    
    기존 결과와 새로 탐색한 경우의 수에 대한 `[이모티콘 플러스 가입자 수, 사용자의 총 구매액]` 결과값을 비교한다.
    

### **어려웠던 지점**

다행히 구현 후, 간단한 예외를 해결한 뒤에는 문제가 바로 해결됐다.

1. **이모티콘의 할인 비율은 `10%` , `20%` , `30%` , `40%` 가 있는데, 이를 `int[]` 타입으로 선언한 것**
    
    이로 인해 비율이 적용된 이모티콘의 구매 가격을 정하는 것에 시간이 좀 걸렸다.
    
2. **완전탐색의 조건을 `if (depth == 7)` 로 설정한 것**
    
    어려웠다기보다는 실수였다. 
    
    처음에 코드 실행할 때 `ArrayIndexOutofBounds` 예외가 떴지만 어렵지 않게 발견할 수 있었다.
    
    문제에서 최대 입력값이 7인 거고, 실제로는 이모티콘의 개수 (`emoticons.length`)로 해야 했다.
    

### **해결 방법**

간단하게 해결할 수 있었다.

1. **할인 비율 `10%`, `20%`, `30%`, `40%` 를 `double[]` 타입으로 선언**
    
    → `double[] rates = {0.1, 0.2, 0.3, 0.4}` 
    
    그리고 이모티콘 가격 계산 시, 타입 캐스팅으로 할인 금액을 계산하는 로직으로 수정함으로써 해결했다.
    
2. **`if (depth == emotiocons.length)` 로 해결함으로써 간단하게 해결**

### **시간복잡도 / 공간복잡도**

- 시간복잡도: $O(4^{m} \times n \times m)$ (1 ≤ m ≤ 7, 1 ≤ n ≤ 100)
    
- 공간복잡도: $O(m)$ (1 ≤ m ≤ 7) (`selectedRates` 배열 + 재귀 스택 깊이)

### **새롭게 알게 된 점**

이번 문제는 코드를 다 작성한 뒤, 테스트케이스와 실행 결과가 달라서 코드를 수정하는 작업이 없었다.

→ 원래 위 과정에서 시간을 굉장이 많이 뺏겼는데, 그래서 목표 시간 내에 문제를 풀 수 있었던 것 같다.

주석으로 어떻게 문제를 풀어나갈지 전체적으로 설계한 뒤, 코드로 작성해나가는 방식 덕분인 것 같다.

그리고 다음부터는 비율을 계산하는 경우에는 `double` , 혹은 `float` 타입을 사용하는 것을 잊지 말아야겠다.

---

## 문제 2

### 문제 정보

- 문제명:
- 문제 링크:
- 사용한 알고리즘 / 자료구조:

### 처음 접근 방식



### 풀이 방법



### 어려웠던 지점



### 해결 방법



### 시간복잡도 / 공간복잡도

- 시간복잡도:
- 공간복잡도:

### 새롭게 알게 된 점



---

## 문제 3

### 문제 정보

- 문제명:
- 문제 링크:
- 사용한 알고리즘 / 자료구조:

### 처음 접근 방식



### 풀이 방법



### 어려웠던 지점



### 해결 방법



### 시간복잡도 / 공간복잡도

- 시간복잡도:
- 공간복잡도:

### 새롭게 알게 된 점



---

# 다음 주 목표

- 목표 문제 수:
- 집중할 유형: