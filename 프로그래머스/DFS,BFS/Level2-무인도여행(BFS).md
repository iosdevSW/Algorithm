# 프로그래머스 Level2-무인도여행(BFS) [Swift]

## 🔎 분류 : BFS

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/154540)
<br><br>

## 📝 문제 설명
메리는 여름을 맞아 무인도로 여행을 가기 위해 지도를 보고 있습니다. 지도에는 바다와 무인도들에 대한 정보가 표시돼 있습니다. 지도는 1 x 1크기의 사각형들로 이루어진 직사각형 격자 형태이며, 격자의 각 칸에는 'X' 또는 1에서 9 사이의 자연수가 적혀있습니다. 지도의 'X'는 바다를 나타내며, 숫자는 무인도를 나타냅니다. 이때, 상, 하, 좌, 우로 연결되는 땅들은 하나의 무인도를 이룹니다. 지도의 각 칸에 적힌 숫자는 식량을 나타내는데, 상, 하, 좌, 우로 연결되는 칸에 적힌 숫자를 모두 합한 값은 해당 무인도에서 최대 며칠동안 머물 수 있는지를 나타냅니다. 어떤 섬으로 놀러 갈지 못 정한 메리는 우선 각 섬에서 최대 며칠씩 머물 수 있는지 알아본 후 놀러갈 섬을 결정하려 합니다.
지도를 나타내는 문자열 배열 maps가 매개변수로 주어질 때, 각 섬에서 최대 며칠씩 머무를 수 있는지 배열에 오름차순으로 담아 return 하는 solution 함수를 완성해주세요. 만약 지낼 수 있는 무인도가 없다면 -1을 배열에 담아 return 해주세요.

## # 제한사항
3 ≤ maps의 길이 ≤ 100<br>
3 ≤ maps[i]의 길이 ≤ 100<br>
maps[i]는 'X' 또는 1 과 9 사이의 자연수로 이루어진 문자열입니다.<br>
지도는 직사각형 형태입니다.
<br><br>

### # 입출력 예
|maps|	result|
|---|---|
|["X591X","X1X5X","X231X", "1XXX1"]|	[1, 1, 27]|
|["XXX","XXX","XXX"]|	[-1]|

## 💡 해결 방법
이번 문제는 마스터한 분야라고 유일하게 말할 수 있는 BFS문제여서 쉽게 구할 수 있었습니다.
BFS응용문제중 시작점이 여러개인 BFS유형이죠. 전체 배열을 돌면서 BFS를 돌려주고 BFS에선 합을 구해서 정답 배열에 넣어주면 됩니다~~ 
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ maps:[String]) -> [Int] {
    let n = maps.count
    let m = maps[0].count
    let dx = [1,-1,0,0]
    let dy = [0,0,1,-1]
    var ans = [Int]()
    
    var map = maps.map { $0.map { String($0) } }
    var check = [Array](repeating: [Bool](repeating: false, count: m), count: n)
    
    
    func bfs(_ x: Int, _ y: Int) {
        var queue = [(x,y)]
        check[x][y] = true
        var idx = 0
        var sum = Int(map[x][y])!
        while idx < queue.count {
            let (x,y) = queue[idx]
            idx += 1
            for i in 0..<4 {
                let nx = x + dx[i]
                let ny = y + dy[i]
                if nx >= 0 && nx < n && ny >= 0 && ny < m && !check[nx][ny] && map[nx][ny] != "X" {
                check[nx][ny] = true
                sum += Int(map[nx][ny])!
                queue.append((nx,ny))
            }
            }
            
        }
        ans.append(sum)
    }
    
    for i in 0..<n {
        for j in 0..<m {
            if map[i][j] == "X" {
                check[i][j] = true
            }
            
            if !check[i][j] {
                bfs(i,j)
            }
        }
    }
    
    return ans.isEmpty ? [-1] : ans.sorted(by: <)
}
```
