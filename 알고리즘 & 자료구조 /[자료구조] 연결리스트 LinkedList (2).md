# 연결리스트 | Linked List

### 리스트에서 값 제거하기
노드를 제거하는 방법에는 세 가지가 있다.
1. <code>pop</code>: 리스트의 맨 앞의 값 제거
2. <code>removeLast</code>: 리스트의 맨 마지막 값 제거
3. <code>remove(after:)</code>: 리스트의 원하는 값 제거

### pop 연산

리스트의 맨 앞의 값을 제거하는 것은 pop이라고 불린다. 이 연산은 <code>push</code>만큼 쉽다.
<code>LinkedList</code> 구조체 안에 아래와 같은 코드를 추가한다.
```swift
    @discardableResult
    public mutating func pop() -> Value? {
        defer {
            head = head?.next
            if isEmpty {
                tail = nil
            }
        }
        return head?.value
    }
```

pop은 리스트에서 제거된 값을 반환한다. 이 값은 없을 수도 있기 때문에 옵셔널(optional)이다.

head를 노드 바로 다음으로 이동하면 리스트의 첫 번째 노드를 효과적으로 제거할 수 있다. 메서드가 끝나면 ARC는 이전 노드에 더 이상 참조가 연결되지 않으므로 메모리에서 이전 노드를 제거한다. 리스트가 비어 있으면 tail을 nil으로 설정한다.

맨 아래에 다음과 같이 실행코드를 입력한다.
```swift
print("Example of pop")
var list4 = LinkedList<Int>()
list4.push(3)
list4.push(2)
list4.push(1)

print("Before popping list: \(list4)")

let poppedValue = list4.pop()
print("After popping list: \(list4)")
print("Popped value: " + String(describing: poppedValue))

/* 출력값
Example of pop
Before popping list: 1 -> 2 -> 3  
After popping list: 2 -> 3 
Popped value: Optional(1)
*/
```
### removeLast 연산
리스트에서 맨 마지막 값을 제거하는 것은 다소 불편해보인다. tail 노드에 대한 참조가 있음에도 불구하고, 그 노드에 대한 참조가 없이 단순히 제거할 수는 없다. 순회 작업이 필요한데, 아래 코드를 pop 아래 추가한다.
```swift
@discardableResult
    public mutating func removeLast() -> Value? {
        
        // 1번
        guard let head = head else {
            return nil
        }
        
        // 2번
        guard head.next != nil else {
            return pop()
        }
        
        // 3번
        var prev = head
        var current = head
        
        while let next = current.next {
            prev = current
            current = next
        }
        
        // 4번
        prev.next = nil
        tail = prev
        return current.value
    }
```
- 1번: head가 nil값이면, 제거할 것이 없으므로 nil을 반환
- 2번: 리스트가 단 하나의 노드만 있다면, <code>removeLast</code> 연산은 기능적으로 pop과 같아진다. pop 연산은 head와 tail 참조를 업데이트하므로 pop연산을 위임하면 된다.
- 3번: <code>current</code>값이 나올 때까지 계속 노드를 검색한다. <code>current</code>가 리스트의 마지막 노드다.
- 4번: <code>current</code>가 마지막 노드이므로, <code>prev.next</code> 참조를 사용하여 단순히 연결만 끊으면 된다. 또한 tail 참조를 업데이트 해야 한다.

파일 맨 아래에 실행 코드를 입력한다.
```swift
print("Example of removing the last node")
var list5 = LinkedList<Int>()
list5.push(3)
list5.push(2)
list5.push(1)

print("Before removing last node: \(list5)")

let removedValue  = list5.removeLast()

print("After removing last node: \(list5)")
print("Removed value: " + String(describing: removedValue))

/* 출력값
Example of removing the last node
Before removing last node: 1 -> 2 -> 3  
After removing last node: 1 -> 2 
Removed value: Optional(3)
*/
```

<code>removeLast</code> 연산은 리스트의 전체 값들을 순회하므로 시간 복잡도가 __O(n)__ 이다. 꽤나 복잡한 축에 속한다.

### remove(after:) 연산
다음 연산은 리스트 내 특정 포인트 즉, 원하는 곳에서의 값을 제거할 수 있는 연산이다. <code>insert(after:)</code> 연산과 꽤나 비슷한데, 제거하고 싶은 노드 바로 이전의 노드 값을 찾아서 제거하기만 하면 된다.

![스크린샷 2024-03-05 오후 12 02 41](https://github.com/Kim-leo/TIL/assets/77371366/00c98cde-c4a9-4f7e-ac45-9be4809e33ec)

<code>LinkedList</code> 구조체 안에 다음과 같은 코드를 추가한다.
```swift
@discardableResult
    public mutating func remove(after node: Node<Value>) -> Value? {
        defer {
            if node.next === tail {
                tail = node
            }
            node.next = node.next?.next
        }
        return node.next?.value
    }
```

노드의 연결을 끊는 작업은 <code>defer</code> 클로져 안에서 실행된다. 제거된 노드가 tail 노드인 경우, tail참조가 반드시 업데이트되어야 하므로 주의가 필요하다.

실행 파일을 맨 아래에 작성한다.
```swift
print("Example of removing a node after aparticular node")
var list6 = LinkedList<Int>()
list6.push(3)
list6.push(2)
list6.push(1)

print("Before removing at particular node: \(list6)")

let index = 1
let node = list6.node(at: index - 1)!
let removedValue2  = list6.remove(after: node)

print("After removing at index \(index): \(list6)")
print("Removed value: " + String(describing: removedValue2))

/* 출력값
Example of removing a node after aparticular node
Before removing at particular node: 1 -> 2 -> 3  
After removing at index 1: 1 -> 3 
Removed value: Optional(2)
*/
```

<code>insert(after:)</code> 연산과 마찬가지로 <code>remove(after:)</code> 연산의 시간 복잡도 또한 __O(1)__ 이지만, 특정 노드에 대한 참조가 미리 있어야 한다.

### 성능 분석
위의 연산에 대한 기능과 시간 복잡도는 아래와 같다.

||pop|removeLast|remove(after:)|
|:---:|:---:|:---:|:---:|
|기능|head 값 제거|tail 값 제거|특정 노드 뒤의 값 제거|
|시간 복잡도|O(1)|O(1)|O(1)|
