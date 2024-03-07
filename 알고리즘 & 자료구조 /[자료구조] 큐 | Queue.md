# 큐 | Queue

큐(Queue)는 FIFO(First-in first-out), 선입선출 순서를 사용한다. 

즉, 처음 추가된 요소가 항상 가장 먼저 제거된다. 

큐는 나중에 처리해야 할 요소의 순서를 유지해야 할 때 유용하다.

### 기본 연산
아래와 같이 Queue를 위한 protocol을 만든다.

```swift
public protocol Queue {
    associatedtype Element
    mutating func enqueue(_ element: Element) -> Bool
    mutating func dequeue() -> Element?
    var isEmpty: Bool { get }
    var peek: Element? { get }
}
```

이 프로토콜은 큐를 위한 주요 연산을 설명한다.

- <code>enqueue</code> 큐의 맨 마지막에 요소를 삽입. 연산이 성공했을 경우 true를 반환.

- <code>dequeue</code> 큐의 맨 앞의 요소를 제거 후 그 값을 반환.

- <code>isEmpty</code> 큐가 비어있는지 확인

- <code>peek</code> 큐의 맨 앞의 값을 제거하지 않고 반환.

큐는 값을 제거할 때에는 맨 앞의 값을 제거하고, 값을 삽입할 때에는 맨 마지막 자리에 삽입을 한다. 값을 삽입하거나 제거할 때 그 사이에 있는 값들은 신경쓰지 않는다.

### 큐 동작 방식 예시와 큐를 만드는 4가지 방법
큐의 동작 방식을 알아보는 가장 쉬운 방법은 아래 그림과 같은 예시를 보는 것이다. 
영화 티켓을 사려고 한 줄로 서있는 사람들이 있는 모습은 큐의 동작 방식을 한 눈에 이해할 수 있다. 
<img width="436" alt="스크린샷 2024-03-07 오후 2 48 05" src="https://github.com/Kim-leo/TIL/assets/77371366/34136498-0d10-4c6c-9589-155d947b1fc1">

큐는 아래와 같이 4가지의 방법으로 생성할 수 있다. 하나씩 알아본다.

- 배열(Array) 로 만들기
- 이중 연결리스트(Dobly linked list) 로 만들기
- Ring buffer 로 만들기
- 스택 2개(Two stacks) 로 만들기

# 배열 기반 큐 연산
Swift 표준 라이브러리에는 상위 수준의 추상화를 구축하는 데 사용할 수 있는 고도로 최적화된 자료구조의 핵심 세트가 함께 제공되는데, 그 중에 하나가 바로 배열이다. 배열은 연속적이고, 정렬된 요소의 리스트를 담는 자료구조의 형태이다.

<img width="423" alt="스크린샷 2024-03-07 오후 2 54 24" src="https://github.com/Kim-leo/TIL/assets/77371366/e532b284-8a6a-4e8f-9bb6-660cdd765376">

앞에서 작성했던 Queue 프로토콜 코드 밑에 다음과 같이 배열 형태의 큐 구조체를 생성한다.
```swift
public struct QueueArray<T>: Queue {
    private var array: [T] = []
    public init() {}
}
```

Queue 프로토콜을 채택하는 QueueArray 구조체를 생성했는데, 컴파일러가 타입 파라미터로 부터 연관된 타입 요소를 추론해줄 것이다.

```swift
// 1번
public var isEmpty: Bool {
    array.isEmpty
}

// 2번
public var peek: T? {
    array.first
}
```

- 1번: 큐가 비어있는지 확인.

- 2번: 큐의 맨 앞 요소를 반환

위의 두 연산은 모두 시간 복잡도가 __O(1)__ 이다.

### Enqueue
큐의 맨 마지막에 요소를 추가하는 것은 정말 쉽다. 배열 기반이기 때문에 단순히 <code>append</code> 연산을 사용하면 된다.
```swift
public mutating func enqueue(_ element: T) -> Bool {
        array.append(element)
        return true
}
```
요소를 삽입하는 것은 평균적으로 __O(1)__ 의 시간 복잡도를 가지는데, 배열은 마지막 부분이 비어있기 때문이다.

<img width="338" alt="스크린샷 2024-03-07 오후 3 03 18" src="https://github.com/Kim-leo/TIL/assets/77371366/f4bee661-f3f8-44f7-8e51-e8cf29da22cd">

위의 그림에서 볼 수 있듯이, 배열 기반 큐에 Mic를 삽입한 후에, 그 뒤에 두 개의 빈 공간이 있다.

값을 계속 추가하다보면, 결국에는 배열이 꽉 찰 것이다. __할당된 공간보다 더 값을 사용하려고 할 때, 배열은 추가적인 공간을 만들기 위해 크기를 재조정 해야 한다.__ 

<img width="463" alt="스크린샷 2024-03-07 오후 3 05 59" src="https://github.com/Kim-leo/TIL/assets/77371366/503b100f-993e-473e-9124-5aefe92a4044">

크기를 재조정하는 것에 대한 시간 복잡도는 __O(n)__ 인 것에 반해, 값을 추가하는 것은 __O(1)__ 이다. 배열의 크기를 재조정할 때엔,ㄴ 새로운 메모리 공간을 할당해야 하고, 새로운 배열에 기존의 데이터를 모두 복사해야 하기 때문이다. 그나마 다행인 것은 이 작업은 꽤 자주 발생하는 것은 아니다. 배열의 공간이 부족해질 때마다 용량이 두 배가 되기 때문이다.

연산에 대한 상각비(평균비)에 대해서는 값을 삽입(enqueue)하는 것은 __O(1)__ 이고, 가장 최악의 시나리오일 때 즉, 모든 데이터가 복사되어야 할 때는 __O(n)__ 이다.

### Dequeue
배열 기반의 큐에서 맨 앞의 값을 제거하는 것은 다음과 같다.

```swift
public mutating func dequeue() -> T? {
        isEmpty ? nil : array.removeFirst()
}
```
큐가 비어있으면 nil을 반환하고, 그렇제 않다면 첫 번째 요소를 제거하고 그 값을 반환한다.

<img width="406" alt="스크린샷 2024-03-07 오후 3 20 41" src="https://github.com/Kim-leo/TIL/assets/77371366/09ca68b4-2526-4387-8c15-27ef12e56cfc">

이는 __O(n)__ 의 시간 복잡도를 가진다. 항상 선형 시간의 연산이 걸리는 이유는 맨 앞의 값을 제거할 때 메모리 상에서 모든 요소들이 한 칸씩 앞으로 이동해야하기 때문이다.

### Debug and test
디버깅을 위해서 아래 코드를 추가한다.
```swift
extension QueueArray: CustomStringConvertible {
    public var description: String {
        String(describing: array)
    }
}

var queue = QueueArray<String>()
queue.enqueue("Ray")
queue.enqueue("Brian")
queue.enqueue("Eric")
print(queue)
queue.dequeue()
print(queue)
print(queue.peek)
/* 출력값
["Ray", "Brian", "Eric"]
["Brian", "Eric"]
Optional("Brian")
*/
```
### 배열 기반 큐의 장단점
|연산|Average case|Worst case|
|:---:|:---:|:---:|
|enqueue|O(1)|O(n)|
|dequeue|O(n)|O(n)|
|Space Complexity|O(n)|O(n)|

사실 구현에는 몇 가지 단점이 있다. __큐의 앞쪽에서 요소를 제거하면 모든 요소가 하나씩 이동하므로 비효율적일 수 있다.__ 이는 큐의 크기가 커질수록 심해지기도 한다. 

__또한 배열이 가득 찼을 때 크기를 조정해야 하는데 그로 인해 사용하지 않은 공간이 메모리 상에서 낭비될 수 있기도 하다.__ 그러면 당연히 메모리 사용량이 증가할 수 있다. 
