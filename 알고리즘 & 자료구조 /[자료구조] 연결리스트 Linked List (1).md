# 연결리스트 | Linked List

연결리스트는 선형, 단방향 배열로 배열된 값들의 집합이다. 연결리스트는 스위프트 배열과 같은 연속적인 저장형태의 옵션에 비해 아래와 같은 이론적인 장점이 있다.

• 값을 삽입하고 제거하는데 시간 복잡도가 O(1)이다.

• 성능의 신뢰도가 높다.

<img width="249" alt="스크린샷 2024-03-04 오후 12 32 27" src="https://github.com/Kim-leo/TIL/assets/77371366/7f621a43-d2ba-4620-bbf1-28989edbeeb4">

위 그림에서 볼 수 있듯이 연결리스트는 연속된 노드(node)들로 이루어져있는데, 노드는 두 가지 책임이 있다.
1. 값을 포함해야 한다.
2. 다음 노드에 대한 참조가 있어야 한다. <code>nil</code> 값은 연결리스트의 끝을 나타낸다.

<img width="218" alt="스크린샷 2024-03-04 오후 12 34 36" src="https://github.com/Kim-leo/TIL/assets/77371366/2c55c7a1-8c2f-4096-a212-751b674970e2">

### 노드 직접 코딩하기 | Node
```swift
public class Node<Value> {
    public var value: Value
    public var next: Node?
    
    public init(value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    public var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> " + String(describing: next) + " "
    }
}
```
그리고 바로 아래에 직접 노드를 선언하고 값을 추가한다.

```swift
print("Exmaple of creating and linking nodes")
let node1 = Node(value: 1)
let node2 = Node(value: 2)
let node3 = Node(value: 3)
node1.next = node2
node2.next = node3
print(node1)

/* 출력값
Exmaple of creating and linking nodes
1 -> 2 -> 3 
*/
```

<img width="282" alt="스크린샷 2024-03-04 오후 12 44 49" src="https://github.com/Kim-leo/TIL/assets/77371366/dfc76cf4-2855-4a4c-a669-d2a64d5fd3e5">

세 개의 노드를 선언하고 그들을 연속되게 이었다. 

사실 위의 방법처럼 리스트를 만들고 값을 추가하기에는 현실적으로 부족한 점이 많다. 값이 많아질수록, 리스트 값이 커질수록 위 방법은 비현실적이다. 이 문제를 완화하기 위해서는 노드 객체를 관리하는 <code>LinkedList</code>를 구축해야 한다.

### 연결리스트 | LinkedList

위의 코드 아래에 다음과 같이 연결리스트를 직접 작성한다.

```swift
public struct LinkedList<Value> {
    public var head: Node<Value>?
    public var tail: Node<Value>?
    
    public init() {}
    
    public var isEmpty: Bool {
        head == nil
    }
}

extension LinkedList: CustomStringConvertible {
    public var description: String {
        guard let head = head else {
            return "Empty list"
        }
        return String(describing: head)
    }
}

```

연결리스트는 __head__ 와 __tail__ 이 있는데, 이는 각각 리스트의 노드의 첫 번째 값과 마지막 값을 의미한다.

<img width="225" alt="스크린샷 2024-03-04 오후 12 50 06" src="https://github.com/Kim-leo/TIL/assets/77371366/b2661170-f6a8-49d0-9c7c-192289f64786">

### 연결리스트에 값 추가하기

앞서 언급했듯이 노드 객체를 관리하기 위한 인터페이스를 알아보자. 

먼저 값을 추가하는 메소드이다. 연결리스트에 값을 추가하는 방법은 세 가지이며, 각각 고유한 성능 특성을 가진다.

1. <code>push</code>: 리스트의 맨 앞에 값을 추가
2. <code>append</code>: 리스트 끝에 값을 추가
3. <code>insert(after:)</code>: 특정 리스트 노드 뒤에 값을 추가

### push 연산
<code>push</code> 연산은 리스트의 맨 앞에 값을 추가한다. __head-first insertion__ 이라고도 한다.

<code>LinkedList</code> 구조체 안에 다음과 같은 코드를 추가한다.
```swift
public mutating func push(_ value: Value) {
        head = Node(value: value, next: head)
        if tail == nil {
            tail = head
        }
    }
```

만약 값이 비어있는 리스트에 push 한다면, 그 새로운 노드는 head인 동시에 tail이 될 것이다.

그리고 다음과 같은 코드로 직접 push 연산을 해보자.

```swift
print("Example of push")
var list = LinkedList<Int>()
list.push(3)
list.push(2)
list.push(1)
print(list)

/* 출력값
Example of push
1 -> 2 -> 3  
*/
```

### append 연산
<code>append</code> 연산은 리스트의 맨 마지막에 값을 추가하는데, __tail-end insertion__ 이라고도 한다.

<code>LinkedList</code> 구조체 안에 다음과 같은 코드를 추가한다.
```swift
public mutating func append(_ value: Value) {
        // 1번
        guard !isEmpty else {
            push(value)
            return
        }
        // 2번
        tail!.next = Node(value: value)

        // 3번
        tail = tail!.next
    }
```

- 1번: 이전과 마찬가지로 리스트가 비어 있으면 새 노드에 head와 tail을 모두 업데이트해야 한다. 빈 리스트에 값을 append하는 것은 push와 기능적으로 동일하므로 push를 호출하여 작업을 수행한다.

- 2번: 다른 모든 경우에는 tail 노드 뒤에 새 노드를 만든다. 위의 guard 문으로 isEmpty 케이스안에 push했기 때문에 force unwrapping이 성공한다.

- 3번: __tail-end insertion__ 이므로 새로운 노드는 리스트의 맨 마지막 tail이 된다.

### insert(after:) 연산
<code>insert(after:)</code> 연산은 특정 위치에 값을 삽입하는 기능을 하는데 아래와 같은 두 가지 단계가 필요하다.
1. 리스트 내에서 특정 노드 값을 찾는다.
2. 새로운 노드를 삽입한다.

<code>LinkedList</code> 구조체 안에 다음과 같은 코드를 추가한다.

```swift
public func node(at index: Int) -> Node<Value>? {
        // 1번
        var currentNode = head
        var currentIndex = 0

        // 2번
        while currentNode != nil && currentIndex < index {
            currentNode = currentNode!.next
            currentIndex += 1
        }
        return currentNode
    }
```

<code>node(at:)</code>는 주어진 인덱스를 기반으로 목록의 노드를 검색한다. head 노드에서부터 리스트의 노드에 액세스할 수 있기 때문에 반복 순회해야 한다.

- 1번: head에 새로운 참조를 만들고 현재 순회 횟수를 추적해야 한다.
- 2번: <code>while</code>루프를 써서, 원하는 인덱스 값에 도달할 때까지 리스트 값들을 차례차례 참조한다. 빈 리스트나 인덱스 범위가 벗어난 경우엔 <code>nil</code> 값을 리턴한다.

이제 새 노드를 삽입할텐데, 아래와 같이 <code>node(at:)</code> 밑에 다음과 같은 메소드를 추가한다.

```swift
// 1번
@discardableResult
    public mutating func insert(_ value: Value, after node: Node<Value>) -> Node<Value> {

        // 2번
        guard tail !== node else {
            append(value)
            return tail!
        }

        // 3번
        node.next = Node(value: value, next: node.next)
        return node.next!
    }
```

- 1번: <code>@discardableResult</code>를 사용하면 컴파일러가 이 메서드에 대해 경고하지 않고도 이 메서드의 반환 값을 무시할 수 있다.
- 2번: 이 메서드를 tail 노드와 함께 호출하는 경우 기능적으로 동등한 추가 메서드를 호출한다. 이를 통해 tail을 업데이트할 수 있다.
- 3번: 그렇지 않으면 새 노드를 리스트의 나머지 부분과 연결하고 새 노드를 반환하기만 하면 된다.

이를 위해 맨 아래에 다음과 같은 코드를 추가한다.
```swift
print("Example of inserting at a particular index")
var list3 = LinkedList<Int>()
list3.push(3)
list3.push(2)
list3.push(1)
print("Before inserting: \(list3)")
var middleNode = list3.node(at: 1)!
for _ in 1...4 {
    middleNode = list3.insert(-1, after: middleNode)
}
print("After inserting: \(list3)")

/* 출력값
Example of inserting at a particular index
Before inserting: 1 -> 2 -> 3  
After inserting: 1 -> 2 -> -1 -> -1 -> -1 -> -1 -> 3
*/
```

위 코드는 기존 연결리스트 안에, 인덱스가 1인 값(2) 바로 뒤에 -1이라는 값을 4번 추가하도록 한다. 값 2와 값 3 사이에 -1이 4번 들어가있는 것을 출력값에서 확인할 수 있다.

### 성능 분석
위의 연산에 대한 기능과 시간 복잡도는 아래와 같다.

||push|append|insert(after:)|node(at:)|
|:---:|:---:|:---:|:---:|:---:|
|기능|head에 값을 추가|tail에 값을 추가|특정 노드 뒤에 값을 추가|주어진 인덱스의 노드를 반환|
|시간 복잡도|O(1)|O(1)|O(1)| O(i), i는 주어진 인덱스|

