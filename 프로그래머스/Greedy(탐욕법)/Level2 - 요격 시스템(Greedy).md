# Level2 - 요격 시스템(Greedy) [Swift]

## 🔎 분류 : Greedy

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/181188)
<br><br>

## 📝 문제 설명
A 나라가 B 나라를 침공하였습니다. B 나라의 대부분의 전략 자원은 아이기스 군사 기지에 집중되어 있기 때문에 A 나라는 B 나라의 아이기스 군사 기지에 융단폭격을 가했습니다.
A 나라의 공격에 대항하여 아이기스 군사 기지에서는 무수히 쏟아지는 폭격 미사일들을 요격하려고 합니다. 이곳에는 백발백중을 자랑하는 요격 시스템이 있지만 운용 비용이 상당하기 때문에 미사일을 최소로 사용해서 모든 폭격 미사일을 요격하려 합니다.
A 나라와 B 나라가 싸우고 있는 이 세계는 2 차원 공간으로 이루어져 있습니다. A 나라가 발사한 폭격 미사일은 x 축에 평행한 직선 형태의 모양이며 개구간을 나타내는 정수 쌍 (s, e) 형태로 표현됩니다. B 나라는 특정 x 좌표에서 y 축에 수평이 되도록 미사일을 발사하며, 발사된 미사일은 해당 x 좌표에 걸쳐있는 모든 폭격 미사일을 관통하여 한 번에 요격할 수 있습니다. 단, 개구간 (s, e)로 표현되는 폭격 미사일은 s와 e에서 발사하는 요격 미사일로는 요격할 수 없습니다. 요격 미사일은 실수인 x 좌표에서도 발사할 수 있습니다.
각 폭격 미사일의 x 좌표 범위 목록 targets이 매개변수로 주어질 때, 모든 폭격 미사일을 요격하기 위해 필요한 요격 미사일 수의 최솟값을 return 하도록 solution 함수를 완성해 주세요.
### # 제한 사항
1 ≤ targets의 길이 ≤ 500,000
targets의 각 행은 [s,e] 형태입니다.
이는 한 폭격 미사일의 x 좌표 범위를 나타내며, 개구간 (s, e)에서 요격해야 합니다.
0 ≤ s < e ≤ 100,000,000
<br><br>

## 💡 해결 방법
비슷한 문제를 풀어본 경험이 있어서 바로 그리디를 생각해낼 수 있었습니다! 역시 많이 풀어봐야 좋은 것 같아요.
1. 개구간 e에 대하여 오름차순으로 정렬을 해줍니다.
2. 가장 먼저 닫히는 개구간e를 변수 t에 담아줍니다. 비교를 하기 위해서 입니다!
3. 정렬된 폭격미사일 Targets을 순회합니다.
4. 순회하면서 t보다 s의 값이 크면 이전에 순회하던 미사일들을 요격 (cnt+1)하고 t값을 갱신해줍니다.
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ targets:[[Int]]) -> Int {
    var cnt = 0
    var t = 0
    var targets = targets.sorted(by: { $0[1] < $1[1]})
    
    t = targets[0][1]
    for target in targets {
        if target[0] >= t {
            t = target[1]
            cnt += 1
        }
    }
    return cnt + 1
}
```
