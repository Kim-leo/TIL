# Cocoa MVC(Model - View - Controller) 소프트웨어 디자인 패턴

### 01. Model - View - Controller
MVC는 User Interface로부터 로직을 분리하여 App의 시각적 요소나 백그라운드에서 실행되는 로직을 서로 영향 없이 쉽게 관리가능한 패턴이다. MVC에서 Model, View, Controller는 다음과 같다.
> Model: 데이터 관리

> View: 텍스트, 이미지 등 User Interface

> Controller: 데이터와 비즈니스 로직 사이의 상호작용 (Model - View 중간 역할)

### 02. 세 객체가 상호작용하는 방법
![MVC패턴이미지](https://github.com/Kim-leo/TIL/assets/77371366/59843cab-9237-4674-844b-0ae16b581208)
1. 사용자가 __View__ 내의 UIComponents 입력 또는 터치 등의 __Event__ 발생
2. 사용자의 Event를 __Controller__ 객체가 받은 후 __Model을 업데이트__
3. __Model__ 은 해당 작업을 위한 __로직 수행__ 후, 다시 Controller에게 알림
4. Model로부터 알림받은 __Controller__ 는 해당 데이터와 로직 결과 등을 __View에 업데이트__
5. 사용자는 입력에 대한 결과를 확인

### 03. MVC 장단점
#### 장점
Controller가 Model과 View의 중간다리 역할을 하기 때문에 Model과 View 사이의 직접적인 상호작용 또는 간섭이 없다. Model과 View는 각각 별개의 객체로서 역할을 수행한다. 이는 다음과 같은 장단점을 제공한다.
1. 단순함과 사용 용이성

View, Model, Controller 세 객체로만 정의되어 구성요소가 적으며, 도입 및 유지 관리가 간단하다.

2. 재사용성

View와 Model은 서로 다르게 동작하기 때문에 View 따로, Model 따로 버전 관리가 용이하다.

#### 단점
1. 무겁고 복잡할 가능성

주어진 작업이 View 또는 Model에 속하지 않으면 일반적으로 Controller 안에 구현되기 때문에 크고 복잡한 개체가 된다. (그렇기 때문에 Massive View Controller 또는 Fat View Controller 라고도 불린다.)

2. 단위 테스트의 어려움

코드를 테스트 할때, 비즈니스 로직이 단위 테스트로 이루어져야 한다. MVC에서는 이 로직이 View와 Model이 각각 긴밀하게 결합된 복잡하고 무거운 Controller에 속해 있다. 이 때문에 제대로된 단위 테스트가 어려울 수 있다.

3. 테스트 가능성 감소

Controller의 크기와 복잡성이 증가함에 따라 App을 유지 및 개발하는데 문제가 있으며 충분한 단위 테스트로 프로젝트를 관리하는데 어려움이 있다.
