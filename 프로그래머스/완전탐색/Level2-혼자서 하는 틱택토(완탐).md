# [프로그래머스] Level2-혼자서 하는 틱택토(완탐) [Swift]

## 🔎 분류 : 완탐

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/160585)
<br><br>

## 📝 문제 설명
택토는 두 사람이 하는 게임으로 처음에 3x3의 빈칸으로 이루어진 게임판에 선공이 "O", 후공이 "X"를 번갈아가면서 빈칸에 표시하는 게임입니다. 가로, 세로, 대각선으로 3개가 같은 표시가 만들어지면 같은 표시를 만든 사람이 승리하고 게임이 종료되며 9칸이 모두 차서 더 이상 표시를 할 수 없는 경우에는 무승부로 게임이 종료됩니다.
할 일이 없어 한가한 머쓱이는 두 사람이 하는 게임인 틱택토를 다음과 같이 혼자서 하려고 합니다.
혼자서 선공과 후공을 둘 다 맡는다.
틱택토 게임을 시작한 후 "O"와 "X"를 혼자서 번갈아 가면서 표시를 하면서 진행한다.
틱택토는 단순한 규칙으로 게임이 금방 끝나기에 머쓱이는 한 게임이 종료되면 다시 3x3 빈칸을 그린 뒤 다시 게임을 반복했습니다. 그렇게 틱택토 수 십 판을 했더니 머쓱이는 게임 도중에 다음과 같이 규칙을 어기는 실수를 했을 수도 있습니다.
"O"를 표시할 차례인데 "X"를 표시하거나 반대로 "X"를 표시할 차례인데 "O"를 표시한다.
선공이나 후공이 승리해서 게임이 종료되었음에도 그 게임을 진행한다.
게임 도중 게임판을 본 어느 순간 머쓱이는 본인이 실수를 했는지 의문이 생겼습니다. 혼자서 틱택토를 했기에 게임하는 과정을 지켜본 사람이 없어 이를 알 수는 없습니다. 그러나 게임판만 봤을 때 실제로 틱택토 규칙을 지켜서 진행했을 때 나올 수 있는 상황인지는 판단할 수 있을 것 같고 문제가 없다면 게임을 이어서 하려고 합니다.
머쓱이가 혼자서 게임을 진행하다 의문이 생긴 틱택토 게임판의 정보를 담고 있는 문자열 배열 board가 매개변수로 주어질 때, 이 게임판이 규칙을 지켜서 틱택토를 진행했을 때 나올 수 있는 게임 상황이면 1을 아니라면 0을 return 하는 solution 함수를 작성해 주세요.

### # 제한사항
board의 길이 = board[i]의 길이 = 3
board의 원소는 모두 "O", "X", "."으로만 이루어져 있습니다.
board[i][j]는 i + 1행 j + 1열에 해당하는 칸의 상태를 나타냅니다.
"."은 빈칸을, "O"와 "X"는 해당 문자로 칸이 표시되어 있다는 의미입니다.

### # 입출력 예
|board|	result|
|---|---|
|["O.X", ".O.", "..X"]|	1|
|["OOO", "...", "XXX"]|	0|
|["...", ".X.", "..."]|	0|
|["...", "...", "..."]|	1|
<br><br>

## 💡 해결 방법
3x3크기의 보드이기 때문에 시간 복잡도에 대한 고민이 전혀 필요가 없습니다. <br>
완전탐색으로 O의 개수 X의 개수를 구해서 말이 되지 않는 경우를 거르고 <br>
OBingo 일 때 XBingo일 때 X의 개수 O의 개수 중 말이 되지 않는 경우를 거른 후 나머지는 통과입니다.<br>

일단 O가 선공이므로 X가 O보다 많아진다거나 O가 X보다 1개 이상은 많을 수 있지만 2개 이상 많아지는 경우는 말이 되지 않으므로 걸러줍니다.

또 XBingo가 나왔는데 OBingo가 나온다거나 OBingo가 나왔는데 XBingo가 나올 순 없죠. 빙고시 종료되어야하니까요! 이 경우도 걸러줍니다.

OBingo시에는 X의 개수가 O의 개수보다 많거나 같아질 수 없죠 이런 경우도 걸러줍니다.

XBingo시에는 O의 개수가 X의 개수와 같은게 정상이고 더 클 순 없습니다. 이런 경우도 걸러줍니다.

즉 정리하면,<br>
1. O의 개수가 X개수보다 작은 경우
2. O의 개수가 X의 개수보다 2개 이상 많은 경우
3. XBingo YBingo 가 둘 다 하나 이상 나온경우
4. XBingo인데 O의 개수가 X의 개수보다 많은 경우
5. OBingo인데 X의 개수가 O와 같거나 많은경우

의 경우를 제외하면 정상적인 게임상황으로 1을 반환하고 위 경우엔 0을 리턴해주면됩니다.
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ board:[String]) -> Int {
    var arr = [[String]]()
    var oCnt = 0
    var xCnt = 0
    var oBingo = 0
    var xBingo = 0
    
    for row in board {
        arr.append(row.map { String($0) })
        for v in row {
            if v == "O" { oCnt += 1 }
            if v == "X" { xCnt += 1 }
        }
    }
    
    if oCnt < xCnt || oCnt > xCnt+1 { return 0 }
    
    //가로 세로 빙고
    for i in 0..<3 {
        let horizontal = arr[i].joined()
        if horizontal == "OOO" { oBingo += 1 }
        if horizontal == "XXX" { xBingo += 1 }
        
        let vertical = arr[0][i] + arr[1][i] + arr[2][i]
        if vertical == "OOO" { oBingo += 1 }
        if vertical == "XXX" { xBingo += 1 }
    }
    
    //대각 빙고
    let rightDown = arr[0][0] + arr[1][1] + arr[2][2]
    if rightDown == "OOO" { oBingo += 1 }
    if rightDown == "XXX" { xBingo += 1 }
    
    let leftDonw = arr[0][2] + arr[1][1] + arr[2][0]
    if leftDonw == "OOO" { oBingo += 1 }
    if leftDonw == "XXX" { xBingo += 1 }
    
    if oBingo != 0 && xBingo != 0 { return 0 }
    if oBingo == 1 && oCnt <= xCnt { return 0 }
    if xBingo == 1 && oCnt > xCnt { return 0 }
     
    return 1
}
```
