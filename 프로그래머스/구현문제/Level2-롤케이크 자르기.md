# Level2-롤케이크 자르기 

### 🔎 분류 : 구현<br><br>
[문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/132265)
<br>

## 📝 문제 설명
철수는 롤케이크를 두 조각으로 잘라서 동생과 한 조각씩 나눠 먹으려고 합니다. 이 롤케이크에는 여러가지 토핑들이 일렬로 올려져 있습니다. 철수와 동생은 롤케이크를 공평하게 나눠먹으려 하는데, 그들은 롤케이크의 크기보다 롤케이크 위에 올려진 토핑들의 종류에 더 관심이 많습니다. 그래서 잘린 조각들의 크기와 올려진 토핑의 개수에 상관없이 각 조각에 동일한 가짓수의 토핑이 올라가면 공평하게 롤케이크가 나누어진 것으로 생각합니다.
예를 들어, 롤케이크에 4가지 종류의 토핑이 올려져 있다고 합시다. 토핑들을 1, 2, 3, 4와 같이 번호로 표시했을 때, 케이크 위에 토핑들이 [1, 2, 1, 3, 1, 4, 1, 2] 순서로 올려져 있습니다. 만약 세 번째 토핑(1)과 네 번째 토핑(3) 사이를 자르면 롤케이크의 토핑은 [1, 2, 1], [3, 1, 4, 1, 2]로 나뉘게 됩니다. 철수가 [1, 2, 1]이 놓인 조각을, 동생이 [3, 1, 4, 1, 2]가 놓인 조각을 먹게 되면 철수는 두 가지 토핑(1, 2)을 맛볼 수 있지만, 동생은 네 가지 토핑(1, 2, 3, 4)을 맛볼 수 있으므로, 이는 공평하게 나누어진 것이 아닙니다. 만약 롤케이크의 네 번째 토핑(3)과 다섯 번째 토핑(1) 사이를 자르면 [1, 2, 1, 3], [1, 4, 1, 2]로 나뉘게 됩니다. 이 경우 철수는 세 가지 토핑(1, 2, 3)을, 동생도 세 가지 토핑(1, 2, 4)을 맛볼 수 있으므로, 이는 공평하게 나누어진 것입니다. 공평하게 롤케이크를 자르는 방법은 여러가지 일 수 있습니다. 위의 롤케이크를 [1, 2, 1, 3, 1], [4, 1, 2]으로 잘라도 공평하게 나뉩니다. 어떤 경우에는 롤케이크를 공평하게 나누지 못할 수도 있습니다.
롤케이크에 올려진 토핑들의 번호를 저장한 정수 배열 topping이 매개변수로 주어질 때, 롤케이크를 공평하게 자르는 방법의 수를 return 하도록 solution 함수를 완성해주세요.<br><br>
## 💡 해결 방법
첫 번째 풀이. 토핑 배열을 n번 순회하며 하나씩 다른 배열에 추가하고 중복을 제거 후 비교하면 간단하다고 생각하였으나 얼마나 비싼 롤케이크인지 1,000,000 개의 토핑까지 올라갈 수 있어서 시간초과가 났다..  반복을 돌며 중복을 제거할 때 Set 으로 처리하면서 시간이 초과된 것 같다.<br>
두 번째 풀이. 루프문에서 Set을 제거하여 철수와 동생의 Set을 처음에 선언해두고 추가제거하였다. 효율은 조금 올랐으나 루프문을 여러번 돌리고 루프문안에서 서브배열을 처리하여 여전히 시간초과가 많았다.<br>
세 번째 풀이. 철수와 동생 Set을 초기에 선언해두고 토핑 종류의 전체 수를 구한 뒤 루프문에서 조건(전체 토핑이 철수에게 있는 경우 동생은 0개 )에 맞게 추가 제거해주는 방식을 이용하여 해결할 수 있었다.

<br><br>
## 첫 Code 풀이 - 시간 초과
```Swift
import Foundation

func solution(_ topping:[Int]) -> Int {
    var topping = topping
    var broSlice = [Int]()
    var answer = 0
    
    for i in 0..<topping.count-1 {
        broSlice.append(topping.removeFirst())
        
        if Set(broSlice).count == Set(topping).count {
            answer += 1
        }
    }
    
    return answer
}
```
## 두 번째 Code 풀이 - 시간 초과 (조금 향상))
```Swift
import Foundation

func solution(_ topping:[Int]) -> Int {
    var toppingReverse = topping.reversed().map{ Int($0) }
    var slice1 = Set<Int>() // 철수
    var slice2 = Set<Int>() // 동생
    var arr1 = [Int]() // 철수 토핑 개수
    var arr2 = [Int]() // 동생 토핑 개수
    
    var answer = 0
    
    for i in 0..<topping.count-1 {
        let tp1 = topping[i]
        let tp2 = topping[topping.count-1 - i]
        slice1.insert(tp1)
        slice2.insert(tp2)
        arr1.append(slice1.count)
        arr2.insert(slice2.count, at: 0)
    }
    
    for (i,v) in arr1.enumerated() {
        if v == arr2[i] {
            answer += 1
        }
    }
    
    return answer
}
```
## 세 번째 Code 풀이 - 해결
```Swift
import Foundation

func solution(_ topping:[Int]) -> Int {
    var slice1 = Set<Int>() // 철수
    var slice2 = Set(topping) // 동생
    var toppingtypes = [Int](repeating: 0, count: slice2.max()!+1) // 토핑 종류 수
    var answer = 0
    
    // 토핑 종류 카운트
    for i in 0..<topping.count {
        toppingtypes[topping[i]] += 1
    }
    
    for i in 0..<topping.count-1 {
        let tp = topping[i]
        slice1.insert(tp) // 철수 토핑 증가
        toppingtypes[tp] -= 1 // 토핑 개수 감소
        
        if toppingtypes[tp] <= 0 { // 토핑의 개수가 0보다 작으면
            slice2.remove(tp) // 해당 토핑은 동생에게 없다.
        }
        
        if slice1.count == slice2.count { // 개수가 같으면 +1
            answer += 1
        }
    }
    
    return answer
}
```