알고리즘 md 템플릿

# 2 x n 타일링 2

## 🔎 분류 : DP
<br><br>
🔗[문제 원본 링크](https://www.acmicpc.net/problem/11727)
<br><br>
## 📝 문제 설명
2×n 직사각형을 1×2, 2×1과 2×2 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오.

아래 그림은 2×17 직사각형을 채운 한가지 예이다.

<img src = https://www.acmicpc.net/upload/images/t2n2122.gif>

### 입력
첫째 줄에 n이 주어진다. (1 ≤ n ≤ 1,000)
### 출력
첫째 줄에 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력한다.
<br><br>
## 💡 해결 방법
DP유형의 문제임을 알고 풀어서 바로 점화식 먼저 찾아보았다. bottomUP 방식으로 찾기 위해 일정한 규칙이 있나 1부터 4까지 먼저 구해본 결과
dp[i] = dp[i-1] + dp[i-2] + 1 의 규칙을 발견하여 IDE에서 테스트 케이스를 돌려보았으나 맞는 점화식이 아닌 것을 금방 발견 할  수 있었다..
고민 끝에 순열의 규칙을 생각해 볼 수 있었다. n개의 배열로 구할 수 있는 순열의 수는 n!개 이며 1개를 고정하면 n-1! 2개를 고정하면 n-2!개의 순열을 구할 수 있다.<br>
즉 이 경우에는 2xN개 중 2x1 2개로 (n-2)개의 경우 , 2x2 1개 를 통해  n-2개의 경우 즉 2(n-2)의 경우와
2xN개 중 2x1 또는 1x2를 고정하여 구하는 n-1개의 경우의 수를 합하여 구할 수 있었다.

DP 점화식 구하는 방법 겨우 이해했지만 그래도 어렵다.. 최대한 많은 문제를 접하자!! 
<br><br>
## Code 풀이
```swift
let n = Int(readLine()!)!
var dp = Array(repeating: 0, count: n+1)
if 2 < n {
    dp[1] = 1
    dp[2] = 3
    for i in 3...n {
        dp[i] = (2*dp[i-2] + dp[i-1]) % 10007
    }
    print(dp[n])
} else if n == 2 {
    print(3)
} else {
    print(1)
}
```
