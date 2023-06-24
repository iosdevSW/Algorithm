
# [프로래머스] Level2-미로 탈출(BFS) [Swift]

## 🔎 분류 : BFS

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/159993#)
<br><br>

## 📝 문제 설명
1 x 1 크기의 칸들로 이루어진 직사각형 격자 형태의 미로에서 탈출하려고 합니다. 각 칸은 통로 또는 벽으로 구성되어 있으며, 벽으로 된 칸은 지나갈 수 없고 통로로 된 칸으로만 이동할 수 있습니다. 통로들 중 한 칸에는 미로를 빠져나가는 문이 있는데, 이 문은 레버를 당겨서만 열 수 있습니다. 레버 또한 통로들 중 한 칸에 있습니다. 따라서, 출발 지점에서 먼저 레버가 있는 칸으로 이동하여 레버를 당긴 후 미로를 빠져나가는 문이 있는 칸으로 이동하면 됩니다. 이때 아직 레버를 당기지 않았더라도 출구가 있는 칸을 지나갈 수 있습니다. 미로에서 한 칸을 이동하는데 1초가 걸린다고 할 때, 최대한 빠르게 미로를 빠져나가는데 걸리는 시간을 구하려 합니다.
미로를 나타낸 문자열 배열 maps가 매개변수로 주어질 때, 미로를 탈출하는데 필요한 최소 시간을 return 하는 solution 함수를 완성해주세요. 만약, 탈출할 수 없다면 -1을 return 해주세요.

### # 제한사항
5 ≤ maps의 길이 ≤ 100 <br>
5 ≤ maps[i]의 길이 ≤ 100 <br>
maps[i]는 다음 5개의 문자들로만 이루어져 있습니다. <br>
S : 시작 지점 <br>
E : 출구 <br>
L : 레버 <br>
O : 통로 <br>
X : 벽 <br>

시작 지점과 출구, 레버는 항상 다른 곳에 존재하며 한 개씩만 존재합니다.<br>
출구는 레버가 당겨지지 않아도 지나갈 수 있으며, 모든 통로, 출구, 레버, 시작점은 여러 번 지나갈 수 있습니다.

### # 입출력 예
|maps|	result|
|---|---|
|["SOOOL","XXXXO","OOOOO","OXXXX","OOOOE"]|	16|
|["LOOXS","OOOOX","OOOOO","OOOOO","EOOOO"]|	-1|
<br><br>

## 💡 해결 방법
최단거리 탐색 하면 BFS! 그치만 이 문제는 레버를 당겨야 탈출을 할 수 있기 때문에
시작지점(S)에서 레버(L)까지의 최단거리를 구하고 레버(L)에서 탈출지점(E)까지의 최단거리를 구할 수 있습니다. 이 점만 파악하면 사실  BFS 두 번 돌리고 두 최단거리를 더해주면 되기 떄문에 쉽습니다!
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ maps:[String]) -> Int {
    var maps = maps.map { $0.map { String($0) } }
    let n = maps.count
    let m = maps[0].count
    
    let dx = [1,-1,0,0]
    let dy = [0,0,1,-1]
    
    var dLever = 0
    var dGoal = 0
    
    func bfs(_ x: Int, _ y: Int, goal: String)->Int {
        var queue = [(x,y)]
        var check = Array(repeating: Array(repeating: 0, count: m), count: n)
        var board = Array(repeating: Array(repeating: 0, count: m), count: n)
        board[x][y] = 1
        var idx = 0
        
        while idx<queue.count {
            let (x,y) = queue[idx]
            idx += 1
            for i in 0..<4 {
                let nx = x + dx[i]
                let ny = y + dy[i]
                
                if nx>=0 && nx < n && ny>=0 && ny < m && maps[nx][ny] != "X" && board[nx][ny] == 0 {
                    check[nx][ny] = check[x][y] + 1
                    if maps[nx][ny] == goal {
                        return check[nx][ny]
                    }
                    board[nx][ny] = 1
                    queue.append((nx,ny))
                }
            }
        }
        return 0
    }
    
    for i in 0..<maps.count {
        for j in 0..<maps[0].count {
            
            if maps[i][j] == "S" {
                dLever = bfs(i,j, goal: "L")
            }
            
            if maps[i][j] == "L" {
                dGoal = bfs(i,j, goal: "E")
            }
        }
    }
    
    if dLever == 0 || dGoal == 0 {
        return -1
    } else {
        return dLever + dGoal
    }
}
```
