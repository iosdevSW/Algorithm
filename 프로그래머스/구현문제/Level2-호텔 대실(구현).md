

#[프로그래머스] Level2-호텔 대실(구현)[Swift]

## 🔎 분류 : 구현 ? 그리디 ?

<br><br>

🔗 [문제 원본 링크](https://school.programmers.co.kr/learn/courses/30/lessons/155651)
<br><br>

## 📝 문제 설명
호텔을 운영 중인 코니는 최소한의 객실만을 사용하여 예약 손님들을 받으려고 합니다. 한 번 사용한 객실은 퇴실 시간을 기준으로 10분간 청소를 하고 다음 손님들이 사용할 수 있습니다.
예약 시각이 문자열 형태로 담긴 2차원 배열 book_time이 매개변수로 주어질 때, 코니에게 필요한 최소 객실의 수를 return 하는 solution 함수를 완성해주세요.

### # 제한사항
1 ≤ book_time의 길이 ≤ 1,000<br>
book_time[i]는 ["HH:MM", "HH:MM"]의 형태로 이루어진 배열입니다<br>
[대실 시작 시각, 대실 종료 시각] 형태입니다.<br>
시각은 HH:MM 형태로 24시간 표기법을 따르며, "00:00" 부터 "23:59" 까지로 주어집니다.<br>
예약 시각이 자정을 넘어가는 경우는 없습니다.<br>
시작 시각은 항상 종료 시각보다 빠릅니다.<br>

### # 입출력 예
|book_time|	result|
|---|---|
|[["15:00", "17:00"], ["16:40", "18:20"], ["14:20", "15:20"], ["14:10", "19:20"], ["18:20", "21:20"]]|	3|
|[["09:10", "10:10"], ["10:20", "12:20"]]|	1|
|[["10:20", "12:30"], ["10:20", "12:30"], ["10:20", "12:30"]]|3|

### # 입출력 예 설명
입출력 예 #1 <br>
<img src="https://user-images.githubusercontent.com/62426665/199907266-561e3b75-84eb-4da1-930c-a6ac8fa82a79.png">
위 사진과 같습니다.<br>
입출력 예 #2 <br>
첫 번째 손님이 10시 10분에 퇴실 후 10분간 청소한 뒤 두 번째 손님이 10시 20분에 입실하여 사용할 수 있으므로 방은 1개만 필요합니다.<br>
입출력 예 #3<br>
세 손님 모두 동일한 시간대를 예약했기 때문에 3개의 방이 필요합니다.
<br><br>



## 💡 해결 방법
이걸 그냥 구현이라고 해야하나 그리디 라고 해야하나 모르겠습니다.. 제일 먼저 떠오른건 빠른 순서대로 정렬해야겠다. 였고 두 번째는 지금 사용중인 방을 담을 컬렉션을 구현하고 그 크기를 구해야겠다. 그리고 방의 수를 카운트했을때 가장 큰 값을 반환해야겠다?
정도 였습니다. 말그대로 구현했고 쉽게 해결할 수 있었습니다.
<br><br>

## Code 풀이
```Swift
import Foundation

func solution(_ book_time:[[String]]) -> Int {
    var room = [Int]()
    var ans = 0
    func getTimeInterval(_ time: String) -> Int {
        let split = time.split(separator: ":").map{ Int(String($0))! }
        
        return split[0]*60 + split[1]
    }
    
    let sortedTime = book_time.sorted { getTimeInterval($0[0]) < getTimeInterval($1[0]) }
    
    for v in sortedTime {
        let s = getTimeInterval(v[0])
        let d = getTimeInterval(v[1])
        
        if room.isEmpty {
            room.append(d+10)
        } else {
            room = room.filter { $0 > s }
            room.append(d+10)
        }
        ans = max(ans,room.count)
    }
    
    return ans
}
```
