# Level2-뒤에 있는 큰 수 찾기(Stack) [Swift]

## 🔎 분류 : Stack

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/154539)
<br><br>

## 📝 문제 설명
정수로 이루어진 배열 numbers가 있습니다. 배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.
정수 배열 numbers가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요. 단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.

### # 제한사항
4 ≤ numbers의 길이 ≤ 1,000,000
1 ≤ numbers[i] ≤ 1,000,000

## 입출력 예
|numbers|	result|
|---|---|
|[2, 3, 3, 5]|	[3, 5, 5, -1]|
|[9, 1, 5, 3, 6, 2]|	[-1, 5, 6, 6, -1, -1]|

## 입출력 예 설명

### 입출력 예 #1
2의 뒷 큰수는 3입니다. 첫 번째 3의 뒷 큰수는 5입니다. 두 번째 3 또한 마찬가지입니다. 5는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [3, 5, 5, -1]이 됩니다.
### 입출력 예 #2
9는 뒷 큰수가 없으므로 -1입니다. 1의 뒷 큰수는 5이며, 5와 3의 뒷 큰수는 6입니다. 6과 2는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [-1, 5, 6, 6, -1, -1]이 됩니다.

<br><br>

## 💡 해결 방법
이번 문제는 Stack을 이용해 해결 하였습니다. 가장 가까운 수가 Stack을 이용하는 포인트 키가 되었습니다.<br>
단순히 2중 for문 이상 사용하게 되면 입력값이 1000000 까지 되기 때문에 n^2은 시간초과를 무시할 수 없게됨을 주의해야합니다.

우선 결과값을 담을 배열을 -1로 초기화하여 선언, 빈 배열을 선언합니다.
주어진 배열을 한 번 반복하면서 스택이 비어있지 않고 현재 스택의 마지막값이 현재 탐색중인 값보다 작은 경우에 현재 스택에 해당하는 결과자리에 현재 탐색중인 값을 넣어주는 것을 반복합니다.
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ numbers:[Int]) -> [Int] {
    var stack = [Int]()
    var ans = [Int](repeating: -1, count: numbers.count)
    for i in 0..<numbers.count {
        while !stack.isEmpty && numbers[stack.last!] < numbers[i] {
            ans[stack.popLast()!] = numbers[i]
        }
        stack.append(i)   
    }
    
    return ans
}
```
