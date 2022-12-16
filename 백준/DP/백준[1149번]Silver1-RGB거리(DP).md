
# 백준[1149번]Silver1-RGB거리 [Swift]

## 🔎 분류 : DP

<br><br>
🔗[문제 원본 링크](https://www.acmicpc.net/problem/1149)
<br><br>
## 📝 문제 설명
RGB거리에는 집이 N개 있다. 거리는 선분으로 나타낼 수 있고, 1번 집부터 N번 집이 순서대로 있다.

집은 빨강, 초록, 파랑 중 하나의 색으로 칠해야 한다. 각각의 집을 빨강, 초록, 파랑으로 칠하는 비용이 주어졌을 때, 아래 규칙을 만족하면서 모든 집을 칠하는 비용의 최솟값을 구해보자.

1번 집의 색은 2번 집의 색과 같지 않아야 한다.
N번 집의 색은 N-1번 집의 색과 같지 않아야 한다.
i(2 ≤ i ≤ N-1)번 집의 색은 i-1번, i+1번 집의 색과 같지 않아야 한다.
### 입력
첫째 줄에 집의 수 N(2 ≤ N ≤ 1,000)이 주어진다. 둘째 줄부터 N개의 줄에는 각 집을 빨강, 초록, 파랑으로 칠하는 비용이 1번 집부터 한 줄에 하나씩 주어진다. 집을 칠하는 비용은 1,000보다 작거나 같은 자연수이다.

### 출력
첫째 줄에 모든 집을 칠하는 비용의 최솟값을 출력한다.
<br><br>
## 💡 해결 방법
이 문제의 해결법을 스스로 찾기 위해 [코드없는 프로그래밍](https://www.youtube.com/watch?v=rhda6lR5kyQ&list=PLDV-cCQnUlIa0owhTLK-VT994Qh6XTy4v&index=11)님의 유튜브 강의를 많이 돌려보았으나.. 2차원 테이블 부터는 이해하기 힘들어서 고생을 많이했다..
그래도 2차원 DP를 만들어서 각각의 경우의 최소를 저장하는 식의 방법을 통해 해결 할  수 있었다.. 아직은 너무 어려운것 같다 dp..
<br><br>
## Code 풀이
```swift
let n = Int(readLine()!)!
var array = [[Int]]()
for _ in 1...n {
    let temp = readLine()!.split(separator: " ").map { Int(String($0))! }
    array.append(temp)
}
var dp = [[Int]](repeating: [Int](repeating: 0, count: 3), count: n+1)

for i in 1...n {
    dp[i][0] = min(dp[i-1][1],dp[i-1][2]) + array[i-1][0]
    dp[i][1] = min(dp[i-1][0],dp[i-1][2]) + array[i-1][1]
    dp[i][2] = min(dp[i-1][0],dp[i-1][1]) + array[i-1][2]
}
print(dp[n].min()!)
```
