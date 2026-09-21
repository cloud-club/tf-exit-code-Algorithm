
# 10주차 문제 풀이 인증

## 기본 정보

* 이름:
* 목표 문제 수: 3
* 실제 풀이 문제 수: 3

---

## 문제 풀이 목록

| 번호 | 문제 이름 | 난이도   | 링크                                                                       |
| -- | ----- | ----- | ------------------------------------------------------------------------ |
| 1  | 네트워크  | Lv. 3 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/43162) |
| 2  | 방문 길이 | Lv. 2 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/49994) |
| 3  | 체육복   | Lv. 1 | [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862) |

---

# 오답노트

## 문제 1. 네트워크

* **문제명:** 네트워크
* **문제 링크:** https://school.programmers.co.kr/learn/courses/30/lessons/43162
* **알고리즘 / 자료구조:** DFS, 그래프, 방문 배열

### 접근 및 시행착오

컴퓨터들이 서로 연결되어 있는지를 확인해서 하나의 네트워크로 묶을 수 있는 컴퓨터들을 찾아야 했다.

처음에는 각 컴퓨터를 하나씩 확인하면서 연결된 컴퓨터들을 방문 처리하는 방식으로 생각했다.

`computers[i][j] == 1`이면 i번 컴퓨터와 j번 컴퓨터가 연결되어 있으므로, 아직 방문하지 않은 컴퓨터라면 계속 탐색해야 한다.

이 문제에서는 한 컴퓨터에서 연결된 컴퓨터를 따라가다 보면 간접적으로 연결된 컴퓨터도 모두 같은 네트워크가 되기 때문에 DFS를 사용했다.

### 최종 풀이

1. `visited` 배열을 만들어 각 컴퓨터를 방문했는지 확인한다.
2. 0번부터 n-1번까지 컴퓨터를 확인한다.
3. 아직 방문하지 않은 컴퓨터를 발견하면 새로운 네트워크이므로 `answer`를 1 증가시킨다.
4. 해당 컴퓨터부터 DFS를 시작한다.
5. DFS에서는 현재 컴퓨터를 방문 처리한다.
6. 모든 컴퓨터를 확인하면서 현재 컴퓨터와 연결되어 있고 아직 방문하지 않은 컴퓨터가 있다면 재귀적으로 DFS를 호출한다.
7. 모든 컴퓨터를 확인한 후 `answer`를 반환한다.

### 내가 작성한 코드

```java
class Solution {
    int answer = 0;
    
    public int solution(int n, int[][] computers) {
        boolean[] visited = new boolean[n];
        int current = 0;
        
        for(int i=0; i<n; i++){
            if(!visited[i]){
                answer++;
                dfs(i, visited, n, computers);
            }
        }
        
        return answer;
    }
    
    void dfs(int current, boolean[] visited, int n, int[][] computers){
        visited[current] = true;
        
        for (int i = 0; i < n; i++) {
            // 현재 컴퓨터와 연결되어 있고 아직 방문하지 않았다면
            if (!visited[i] && computers[current][i] == 1) {
                dfs(i, visited, n, computers);
            }
        }
    }
}
```

### 복잡도

* 시간복잡도: O(n²)
* 공간복잡도: O(n)

### 핵심 포인트

연결된 컴퓨터들을 하나씩 탐색하면서 이미 방문한 컴퓨터는 다시 탐색하지 않는 것이 핵심이다.

DFS를 한 번 시작하면 해당 컴퓨터와 연결된 모든 컴퓨터를 방문하게 되므로, DFS를 시작하는 횟수가 곧 네트워크의 개수가 된다.

---

## 문제 2. 방문 길이

* **문제명:** 방문 길이
* **문제 링크:** https://school.programmers.co.kr/learn/courses/30/lessons/49994
* **알고리즘 / 자료구조:** HashSet, 좌표 이동

### 접근 및 시행착오

이 문제에서는 단순히 이동 횟수를 세는 것이 아니라 **처음 지나간 길의 개수**를 구해야 한다.

예를 들어 `(0,0) -> (0,1)`로 이동한 뒤 다시 `(0,1) -> (0,0)`으로 이동하면 같은 길을 반대 방향으로 이동한 것이므로 새로운 길로 세면 안 된다.

그래서 출발점과 도착점을 하나의 문자열로 만들어 `HashSet`에 저장했다.

반대 방향으로 이동하는 경우도 같은 길이라는 것을 처리하기 위해

`출발점 -> 도착점`

뿐만 아니라

`도착점 -> 출발점`

도 같이 저장했다.

### 최종 풀이

1. 시작 좌표를 `(0,0)`으로 설정한다.
2. `HashSet`을 만들어 지나간 길을 저장한다.
3. 명령어를 하나씩 확인한다.
4. 명령어에 따라 다음 좌표를 계산한다.
5. 범위를 벗어난 이동이면 무시한다.
6. 범위 안이라면 현재 좌표와 다음 좌표를 이용해 길을 만든다.
7. 반대 방향의 길도 함께 `Set`에 저장한다.
8. 모든 이동이 끝난 후 `Set`의 크기를 2로 나누면 실제 이동한 길의 개수가 된다.

### 내가 작성한 코드

```java
import java.util.*;

class Solution {
    public int solution(String dirs) {

        HashSet<String> set = new HashSet<>();
        
        String[] dir = dirs.split("");
        int x = 0;
        int y = 0;
    
        
        for(String d : dir){
            
            int nx = x;
            int ny = y;
            
            if(d.equals("U")){
                ny++;
            } else if(d.equals("D")){
                ny--;
            } else if(d.equals("R")){
                nx++;
            } else if(d.equals("L")){
                nx--;
            }
            
            if(nx > 5 || ny > 5 || nx < -5 || ny < -5){
                continue;
            }
            
            String path = x + "," + y + "->" + nx + "," + ny;
            String opp_path = nx + "," + ny + "->" + x + "," + y;
            
            set.add(path);
            set.add(opp_path);
            
            x = nx;
            y = ny;
        }
        
        int answer = set.size() / 2;
        
        return answer;
    }
}
```

### 복잡도

* 시간복잡도: O(n)
* 공간복잡도: O(n)

`dirs`의 길이를 n이라고 하면 각 명령어를 한 번씩 확인하기 때문에 시간복잡도는 O(n)이다.

### 핵심 포인트

**이동한 횟수와 실제로 처음 지나간 길의 개수는 다르다.**

특히 `(A → B)`와 `(B → A)`는 서로 다른 이동처럼 보이지만 실제로는 같은 길이므로 하나로 처리해야 한다.

---

## 문제 3. 체육복

* **문제명:** 체육복
* **문제 링크:** https://school.programmers.co.kr/learn/courses/30/lessons/42862
* **알고리즘 / 자료구조:** 그리디, 배열

### 접근 및 시행착오

각 학생이 현재 체육복을 몇 개 가지고 있는지를 배열에 저장하는 방식으로 접근했다.

처음에는 모든 학생이 체육복을 1개 가지고 있다고 생각하고 시작한다.

여벌 체육복이 있는 학생은 2개로 만들고, 체육복을 잃어버린 학생은 1개를 감소시킨다.

이렇게 하면 체육복을 잃어버렸지만 여벌 체육복도 가지고 있는 경우에는 최종적으로 체육복이 1개가 되기 때문에 다른 학생에게 빌려줄 수 없는 상황도 자연스럽게 처리할 수 있다.

그 다음 체육복이 0개인 학생을 순서대로 확인하면서 바로 앞 학생 또는 바로 뒤 학생에게 여벌 체육복이 있는 경우 빌린다.

### 최종 풀이

1. 모든 학생의 체육복 개수를 1로 초기화한다.
2. `reserve`에 있는 학생은 체육복을 1개 추가한다.
3. `lost`에 있는 학생은 체육복을 1개 감소시킨다.
4. 체육복이 0개인 학생을 앞에서부터 확인한다.
5. 바로 앞 학생의 체육복이 2개라면 빌린다.
6. 앞 학생에게 빌릴 수 없다면 바로 뒤 학생의 체육복이 2개인지 확인한다.
7. 체육복을 빌린 학생과 빌려준 학생의 개수를 각각 변경한다.
8. 마지막으로 체육복이 1개 이상인 학생의 수를 계산한다.

### 내가 작성한 코드

```java
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        int[] clothes = new int[n+1];
        
        for(int i=0; i<=n; i++){
            clothes[i] = 1; // 체육복이 있음 1
        }
        
        for(int q : reserve){
            clothes[q]++; //체육복 여벌있음 2
        }
        
        for(int q : lost){
            clothes[q]--; //체육복 잃어버림 -1 
        }
        
        for(int i=1; i<=n; i++){
            if(clothes[i] == 0){ // 체육복 없으면
                if(i>1 && clothes[i-1] == 2){
                    clothes[i-1]--;
                    clothes[i]++;
                }
                else if(i<n && clothes[i+1] == 2){
                    clothes[i+1]--;
                    clothes[i]++;
                }
            }
        }
        
        for(int i=1; i<=n; i++){
            if(clothes[i]>=1) answer++;
        }
        
        
        return answer;
    }
}
```

### 복잡도

* 시간복잡도: O(n)
* 공간복잡도: O(n)

학생 수를 n이라고 하면 체육복 배열을 초기화하고 학생들을 순서대로 확인하기 때문에 O(n)이다.

### 핵심 포인트

`lost`와 `reserve`를 별도로 처리하기보다 **현재 가지고 있는 체육복의 개수**를 배열에 저장하면 여벌 체육복을 가진 사람이 도난당한 경우까지 쉽게 처리할 수 있다.

또한 체육복을 빌려줄 때는 앞 번호 학생을 먼저 확인하고, 불가능하면 뒤 번호 학생을 확인하는 방식으로 그리디하게 처리했다.

---

# 다음 주 목표

* 목표 문제 수: 3
* 집중할 유형: DFS/BFS, 그리디, 자료구조 활용
