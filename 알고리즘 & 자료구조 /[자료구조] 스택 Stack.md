# 스택 | Stack
스택 자료구조는 물리적인 객체 스택과 개념적으로 동일하다. 스택에 항목을 추가할 때는 스택의 맨 위에 항목을 배치하고, 스택에서 항목을 제거할 때는 항상 맨 위에 있는 항목을 제거한다.

# 스택 연산 | Stack operations
스택은 유용하고 매우 간단하다. __스택을 구축하는 주요 목표는 데이터에 액세스하는 방법을 강제하는 것이다.__
스택에 필수적인 작업은 두 가지뿐이다.

• push: 스택의 맨 위에 요소를 추가한다. 

• pop: 스택의 맨 위 요소를 제거한다.

인터페이스를 이 두 가지 작업으로 제한한다는 것은 데이터 구조의 한 면에서만 요소를 추가하거나 제거할 수 있음을 의미한다. 컴퓨터 과학에서 스택은 __LIFO(last-in-first-out)__ 데이터 구조로 알려져 있다. 맨 마지막에 밀려난 요소가 가장 먼저 튀어나오는 원리이다.

스택은 프로그래밍의 모든 분야에서 두드러지게 사용된다.

• iOS는 <code>navigation stack</code>을 사용하여 <code>View Controller</code>를 <code>View</code> 안팎으로 <code>push</code> 하거나 <code>pop</code> 한다.

• 메모리 할당은 아키텍처 레벨에서 스택을 사용하는데, 지역 변수(local variable)에 대한 메모리도 스택을 사용하여 관리한다.

• 미로에서 경로를 찾는 것과 같은 <code>Search and conquer Algorithm</code>은 스택을 사용하여 역추적(backtracking)을 용이하게 한다.

# 직접 만들어보기 | Implementation
```swift
public struct Stack<Element> {
    private var storage: [Element] = []
    
    public init() { }  
}
extension Stack: CustomDebugStringConvertible {
    public var debugDescription: String {
        """
        ----top----
        \(storage.map { "\($0)" }.reversed().joined(separator: "\n"))
        -----------
        """
    }
}
```
여기서는 스택의 백업 저장소를 정의했다. 스택에 적합한 저장소 Type을 선택하는 것이 중요하다. 배열(Array)은 <code>append</code>와 <code>popLast</code>를 통해 한 쪽 끝에서 삽입 및 삭제의 시간 복잡도가 __O(1)__ 이기 때문에 배열을 선언했다. 이 두 가지 메소드(<code>append</code>, <code>popLast</code>)를 사용하면 스택의 LIFO 특성을 쉽게 파악할 수 있다.

<code>CustomDebugStringConvertible protocol</code>에서 요구하는 <code>debugDescription의</code> 함수의 경우 다음 세 가지 작업을 수행한다.

1. <code>storage.map {"\"($0)"}</code>을(를) 통해 요소를 <code>String</code> 매핑하는 배열을 생성한다.

2. <code>reverse()</code>를 사용하여 이전 배열을 뒤집는 새 배열을 만든다.

3. <code>joined(separator:)</code>를 사용하여 배열을 문자열로 바꾸고, <code>"\n"</code>을 사용하여 배열의 요소를 구분한다.

이렇게 하면 디버깅을 위한 스택 type으로 <code>print</code>로 표현할 수 있도록 생성된다.

# push & pop 연산
아래 두 연산을 Stack struct에 추가한다.
```swift
    public mutating func push(_ element: Element) {
        storage.append(element)
    }
    
    @discardableResult
    public mutating func pop() -> Element? {
        storage.popLast()
    }
```
그리고 아래와 같이 stack을 선언 후, 숫자 1부터 4까지 <code>push</code>를 한 후 출력해본다.
```swift
print("example of using a stack")
var stack = Stack<Int>()
stack.push(1)
stack.push(2)
stack.push(3)
stack.push(4)

print(stack)

if let poppedElement = stack.pop() {
    assert(4 == poppedElement)
    print("Popped: \(poppedElement)")
}

/* 출력값
example of using a stack
----top----
4
3
2
1
-----------
Popped: 4
*/
```
__<code>push</code>와 <code>pop</code>은 둘 다 O(1)의 시간 복잡도(Time Complexity)를 가진다.__

# 그 외 연산
스택을 더 쉽게 사용할 수 있는 몇 가지 연산이 있다. 마지막 요소를 출력하는 것과 스택이 비어있는지 확인하는 연산이다.

아래 코드를 <code>Stack struct</code> 안에 추가한다.
```swift
    public func peek() -> Element? {
        storage.last
    }
    
    public var isEmpty: Bool {
        peek() == nil
    }
```
<code>peek()</code> 함수는 스택안의 데이터를 변형시키지 않고 맨 마지막 요소(맨 위 요소)를 확인할 수 있다.

# 더 적은 것이 더 많다. | Less is More
스택에 <code>Swift Collection Protocol</code>을 채택할 수 있을까? 스택의 목적은 데이터에 액세스하는 방법의 수를 제한하는 것인데, <code>Collection</code>과 같은 protocol을 채택하는 것은 <code>iterator와 subscript</code>를 통해 모든 요소를 노출시킴으로써 이 목표에 어긋날 것입니다. 이 경우, 적은 것이 더 많은 것이다.

스택에 엑세스 하는 순서를 보장하기 위해 기존 배열을 가져와 스택으로 변환하는 것이 좋다. 물론 배열 요소를 선회하여 각 요소를 <code>push</code>하는 것도 가능하긴 하다. 한편, 기본 <code>private storage</code>를 설정하는 <code>initialzier</code>를 작성할 수 있다. 아래 코드를 <code>Stack struct</code> 안에 추가한다.
```swift
    public init(_ elements: [Element]) {
        storage = elements
    }
```
이제 아래 코드를 실행하여 결과를 확인해보자.
```swift
print("example of initializing a sack from an array")
let array = ["A", "B", "C", "D"]
var stack2 = Stack(array)
print(stack2)
stack2.pop()
print(stack2)

/* 출력값
example of initializing a sack from an array
----top----
D
C
B
A
-----------
----top----
C
B
A
-----------
*/
```
위 코드는 <code>String</code> 형태의 스택을 만들고 맨 마지막 요소인 <code>"D"</code>를 제거한다. __Swift compiler__ 는 배열에서 type 추론이 가능하므로 <code> __Stack<String>__ </code>을 안 쓰고 <code> __var stack2 = Stack(array)__ </code> 이렇게만 써도 가능하다.

뿐만 아니라, <code> __arrayLiteral__ </code>에서 stack을 초기화할 수 있는데 아래 코드를 추가해보자.
```swift
extension Stack: ExpressibleByArrayLiteral {
    public init(arrayLiteral elements: Element...) {
        storage = elements
    }
}
```
그리고 아래 코드도 실행해보면 다음과 같다.
```swift
print("example of initializing a stack from an array literal")
var stack3: Stack = [1.0, 2.0, 3.0, 4.0]
print(stack3)
stack3.pop()
print(stack3)

/* 출력값
example of initializing a stack from an array literal
----top----
4.0
3.0
2.0
1.0
-----------
----top----
3.0
2.0
1.0
-----------
*/
```
이렇게 하면 <code>Double type</code>의 스택이 생성되고 맨 마지막 값 <code>4.0</code>이 <code>pop</code>된다. 다시 말해서, type 추론을 사용하면 더 자세한 <code>Stack<Double></code>을 입력할 필요가 없다.

스택은 트리와 그래프를 탐색하는 문제에서 매우 중요하다. 미로를 통해 길을 찾는 것을 예시로 들자면, 왼쪽, 오른쪽 또는 직선의 결정점에 도달할 때마다 가능한 모든 결정을 스택에 <code>push</code>할 수 있다. 막다른 길에 도달했을 때 스택에서 <code>pop</code>하거나 다른 막다른 길에 부딪힐 때까지 계속해서 되돌아가기만 하면 된다.

# 요점 정리
• 스택은 __LIFO, Last-in First-out__, 자료구조이다.

• 스택은 매우 간단하면서도 핵심적인 자료구조 형태이다.

• 스택에 필수적인 두 가지 작업은 요소를 추가하는 <code>push</code> 방법과 요소를 제거하는 <code>pop</code> 방법뿐이다.
