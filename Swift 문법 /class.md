# class

### 01. class 선언하는 방법

```swift
class 클래스 이름: 부모 클래스 {
    클래스 프로퍼티(변수)
    초기화 메서드(함수)
}
```

Cafe class를 만든다고 할 때, 카페에 포함되는 정보는 위치, 유명한 메뉴, 그리고 리뷰점수, 카페에 대한 정보를 출력하는 cafeInfo 메서드가 있다고 한다면, 다음과 같이 프로퍼티와 초기화 메서드를 작성할 수 있다. 

```swift
class Cafe {
    var location: String
    var famousMenu: String
    var reviewScore: Float
    
    init(location: String, famousMenu: String, reviewScore: Float) {
        self.location = location
        self.famousMenu = famousMenu
        self.reviewScore = reviewScore
    }
    
    deinit {
        print("deleted from Memory.")
    }
    
    func cafeInfo() {
        print("Location: \(location) \n"
        + "Famous menu: \(famousMenu) \n"
        + "Review Score: \(reviewScore)")
    }
}
```
### 02. Instance로 class 사용하기
Cafe class를 사용하기 위해서는 인스턴스를 생성하여 사용 가능하다. 인스턴스를 생성할 때에는 class 내의 초기화 메서드의 파라미터를 전달하여 생성한다.

```swift
let starbucks = Cafe(location: "Seoul", famousMenu: "Iced americano", reviewScore: 4.7)
print(starbucks.location) // "Seoul"
starbucks.cafeInfo() 
/*
  Location: Seoul
  Famous menu: Iced americano
  Review Score: 4.7
*/
```

class 인스턴스(위에서는 starbucks)가 생성되면 메모리에 적재되는데, Swift는 메모리 할당 작업을 ARC(Automatic Reference Counting)에서 자동으로 진행한다. 인스턴스에 대하 참조 코드가 없어지면 메모리에서 제거된다. 이 때 인스턴스가 소멸될 때 돌아갈 수 있게 하는 코드 블럭이 deinit{} 이다.

### 03. 저장 프로퍼티(Stored Property) 와 연산 프로퍼티(Computed Property) 
앞서 클래스를 선언할 때 필요한 것이 데이터 저쟝 역할을 하는 프로퍼티임을 확인했다. 클래스 내에서 사용하는 프로퍼티는 두 가지가 있다. 
> 저장 프로퍼티(Stored Property): 상수, 변수에 저장된 값

> 연산 프로퍼티(Computed Property): 값을 설정하거나 호출하는 시점에서 로직이 실행되는 값


