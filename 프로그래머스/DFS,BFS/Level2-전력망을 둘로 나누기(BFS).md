
# [프로그래머스] Level2-전력망을 둘로 나누기 [Swift]

## 🔎 분류 : BFS

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/86971)
<br><br>

## 📝 문제 설명
n개의 송전탑이 전선을 통해 하나의 트리 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.
송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.

### # 제한사항
n은 2 이상 100 이하인 자연수입니다.
wires는 길이가 n-1인 정수형 2차원 배열입니다.
wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
1 ≤ v1 < v2 ≤ n 입니다.
전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.
<br><br>

## 💡 해결 방법
해당 문제는 양방향 트리이므로 입력받은 wires를 양쪽에 넣어주어야합니다. 트리구조는 딕셔너리를 이용해도 되지만 저는 배열을 이용해 0부터 N+1까지 사용하여 입력받았습니다.

bfs를 Queue를 통해 구현해주면 되고
문제의 전력을 차단하는 방법은 1부터 N까지 돌면서 방문체크를 미리 해둔 후 돌려준다면 방문하지 않게되므로 차단하는것과 다를 것이 없습니다.

이 때 그냥 for문을 돌리게 됐을때 1번 노드를 차단하고 1번 부터 탐색을 하게되는 코드를 짜시게 된다면 결과값을 볼 수 없게됩니다. 계속 탐색을 못하고 종료되니까요!
그래서 저는 n이 2이상이라는 조건을 이용해 미리 1번 노드를 차단했을 경우를 돌려준 뒤 반복문에서 1번 노드를 시작점으로 bfs를 반복하여 위 같은 상황을 방지하였습니다.
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ n:Int, _ wires:[[Int]]) -> Int {
    var tree = Array(repeating: [Int](), count: n+1)
    var visited = Array(repeating: true, count: n)
    var cnt = 0
    var ans = n
    
    for wire in wires {
        tree[wire[0]].append(wire[1])
        tree[wire[1]].append(wire[0])
    }
    
    func bfs(_ start: Int) {
        var queue = [start]
        var idx = 0
        
        while idx < queue.count {
            let last = queue[idx]
            idx += 1
            if visited[last-1] {
                visited[last-1] = false
                queue.append(contentsOf: tree[last])
                cnt += 1
            }
        }
    }
    visited[0] = false
    bfs(2)
    ans = min(ans, abs(cnt - (n - cnt)))
    
    for i in 1..<n {
        visited = Array(repeating: true, count: n)
        visited[i] = false
        cnt = 0
        bfs(1)
        
        ans = min(ans, abs(cnt - (n - cnt)))
    }
    
    return ans
}
```