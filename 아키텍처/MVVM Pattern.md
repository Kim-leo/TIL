# MVVM (Model-View-View Model) 소프트웨어 디자인 패턴

### 01. Model - View - View Model
MVVM 패턴은 Cocoa MVC와 마찬기다로 데이터와 Interface를 Model과 View 객체로 분리한다. 
Controller가 두 패턴 모두에 존재하는 반면, MVVM에서는 Controller의 책임이 줄어든다. 
분리된 비즈니스 로직 코드는 View Model 이라는 새로운 객체에서 수행된다.

> Model: 데이터 저장을 담당하며 View는 이를 인식할 수 없어야 한다. View의 필요에 맞게 데이터를 조작해서는 안 된다.

> View: User에게 데이터를 제공하는 일을 처리하고 User가 수행한 작업을 수신하는 역할을 한다. View 또한 Model의 어떤 방식에 의해서라도 인식될 수 없다. 그렇기 때문에 View에서는 비즈니스 논리가 없다.

> View Model: Model과 Controller 사이의 새로운 계층이다. 이는 Model을 소유하고 데이터를 단순화된 View에 표시할 수 있도록 하는 방식으로 데이터를 조작한다.

> Controller: 소유하고 있는 객체를 설정하고 조정하는 책임을 그대로 유지한다. Model의 데이터를 조작하는 것과 관련된 비즈니스 로직이 View Model 계청으로 이동하기 때문에 MVC에 비해 Controller 역할이 줄어든다. 또한 Model을 직접 소유하는 것이 아닌, View Model을 소유한다.

### 02. 각 개체가 상호작용하는 방법
<img width="529" alt="스크린샷 2024-02-24 오후 11 19 58" src="https://github.com/Kim-leo/TIL/assets/77371366/55bc86c7-e80a-4f75-b923-bd47b9a77484">

1. 사용자가 View 내의 UIComponents 입력 또는 터치 등의 Event 발생
2. View는 해당 Event를 View Model에게 전달
3. 변경된 Data를 통해 Model을 업데이트
4. Model 변경됨을 해당 Model을 소유한 View Model이 인지함과 동시에 View Model 과 binding된 View 업데이트

### 03. MVVM 장점
1. 재사용성

View와 Model 이 명확하게 구분되며, 서로 밀접하게 연결되어 있지 않으므로 쉽게 재사용 할 수 있다. 

2. 테스트 가능성 향상

MVC와 달리, 비즈니스 로직이 View Model로 이동되어 Model의 데이터를 복사하고 View Model의 프로퍼티와 메소드를 쉽게 확인할 수 있다.

3. 향상된 확장성 & 줄어든 복잡성

App의 크기와 기능, 사용자 흐름 수가 증가함에 따라 내부 구조의 복잡성이 MVC 패턴에 비해 더 오랫동안 완화된다. 
이는 특히 Delegate 패턴, Binding, Callback을 사용할 수 있는 경우에 따라서 View Model과의 상호작용 방식을 선택할 때 용이하다.
이러한 확정상은 Controller에서 App의 네비게이션 로직을 추출하기 위해 Coordinator를 도입함으로써 더욱 향상 가능하다. (이 새로운 패턴을 MVVM-C 라고 한다.)


