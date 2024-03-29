# 백준[11057] 오르막 수 (Swift)

## 🔎 분류 : DP
<br><br>
🔗[문제 원본 링크]( )
<br><br>
## 📝 문제 설명
오르막 수는 수의 자리가 오름차순을 이루는 수를 말한다. 이때, 인접한 수가 같아도 오름차순으로 친다.

예를 들어, 2234와 3678, 11119는 오르막 수이지만, 2232, 3676, 91111은 오르막 수가 아니다.

수의 길이 N이 주어졌을 때, 오르막 수의 개수를 구하는 프로그램을 작성하시오. 수는 0으로 시작할 수 있다.

### 입력
첫째 줄에 N (1 ≤ N ≤ 1,000)이 주어진다.

### 출력
첫째 줄에 길이가 N인 오르막 수의 개수를 10,007로 나눈 나머지를 출력한다.
<br><br>
## 💡 해결 방법
각 자리수 만큼의 반복을 통해 앞자리보다 크거나 같은 수의 모든 수를 더하는 경우 이므로
n-1번 반복과 함께 각 자리수의 경우를 더하고 마지막의 모든 자리수dp를 더함으로써 가질 수 있는 총 확률을 구할 수 있었다.
<br><br>
## Code 풀이

```Swift
let n = Int(readLine()!)!
var dp = [Int](repeating: 1, count: 10)
for _ in 1..<n {
    for i in 1...9 {
        dp[i] = (dp[i-1] + dp[i]) % 10007
    }
}
print(dp.reduce(0,+) % 10007)
```
