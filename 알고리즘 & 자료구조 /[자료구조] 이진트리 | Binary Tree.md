# 이진 트리 | Binary Tree

트리의 기본 구조는 여러 자식 노드를 가질 수 있는 형태이다. 

__이진 트리(Binary Tree)__ 는 딱 2개의 자식 노드만 가지는 트리이다. 자식 노드는 왼쪽, 오른쪽으로 구분한다.

<img width="176" alt="스크린샷 2024-03-13 오후 6 41 58" src="https://github.com/Kim-leo/TIL/assets/77371366/a9ba4f9c-1009-4110-a0af-84477a1b573c">

이진트리는 트리 자료구조와 알고리즘의 기본이 된다.

### 코드로 이진트리 만들기
아래와 같이 직접 이진 트리 <code>class</code> 를 만든다.
```swift
public class BinaryNode<Element> {
    public var value: Element
    public var leftChild: BinaryNode?
    public var rightChild: BinaryNode?

    public init(value: Element) {
        self.value = value
    }
}
```

맨 아래에 다음과 같이 코드를 추가한다.
```swift
var tree: BinaryNode<Int> = {
    let zero = BinaryNode(value: 0)
    let one = BinaryNode(value: 1)
    let five = BinaryNode(value: 5)
    let seven = BinaryNode(value: 7)
    let eight = BinaryNode(value: 8)
    let nine = BinaryNode(value: 9)

    seven.leftChild = one
    one.leftChild = zero
    one.rightChild = five
    seven.rightChild = nine
    nine.leftChild = eight

    return seven
}()
```

아래 그림과 같은 이진 트리를 위 코드로 만들어 본 것이다.

<img width="142" alt="스크린샷 2024-03-13 오후 6 46 49" src="https://github.com/Kim-leo/TIL/assets/77371366/71a07e4e-270d-4995-b107-760685c44f5c">

### 다이어그램 만들기
자료구조의 기본 모델을 만들다보면 이의 작동 방식을 배우는 것에 많은 도움이 된다. 이진 트리를 시각화하는 재사용 가능한 알고리즘을 직접 만들어보자.

아래에 다음과 같은 코드를 입력한다.
```swift
extension BinaryNode: CustomStringConvertible {
    public var description: String {
        diagram(for: self)
    }
    
    private func diagram(for node: BinaryNode?, _ top: String = "", _ root: String = "", _ bottom: String = "") -> String {
        guard let node = node else {
            return root + "nil \n"
        }
        if node.leftChild == nil && node.rightChild == nil {
            return root + "\(node.value) \n"
        }
        return diagram(for: node.rightChild,
                       top + " ", top + "┌──", top + "│ ")
                        + root + "\(node.value)\n"
                        + diagram(for: node.leftChild,
                                  bottom + "│ ", bottom + "└──", bottom + " ")
    }
}
```

<code>diagram</code>은 이진 트리를 반복하는 <code>String</code>을 만든다. 이제 아래에 직접 <code>tree</code> 를 출력해본다.

```swift
print("Example of tree diagram")
print(tree)
/* 결과값
Example of tree diagram
 ┌──nil 
┌──9
│ └──8 
7
│ ┌──5 
└──1
 └──0 
*/
```

### 순회 알고리즘
이전에 트리의 레벨 순회(Level-order traversal)을 살펴보았다. 이를 살짝 조정하여 이진 트리에도 동작하는 똑같은 알고리즘을 만들 . 수있다. 대신, 레벨 순회 알고리즘을 재실행하지 않고, 아래 다음 세 가지 순회 알고리즘을 살펴보아야 한다.

- 중위 순회(in-order)
- 전위 순회(pre-order)
- 후위 순회(post-order)

### 중위 순회 | In-order traversal
중위 순회는 이진 트리의 노드들을 루트노드를 시작으로 하여 아래와 같이 방문한다. 

- 현재 노드가 왼쪽 자식 노드가 있으면 연속적으로 그 자식 노드를 첫번째로 방문한다.
- 그 다음, 그 노드 자신을 방문한다.
- 현재 노드가 오른쪽 자식 노드가 있으면, 그 자식 노드를 연속적으로 방문한다.


<img width="220" alt="스크린샷 2024-03-13 오후 7 06 45" src="https://github.com/Kim-leo/TIL/assets/77371366/e2834717-a85e-41a1-a515-9107d3ffa86f">

이는 오름차순으로 위의 노드를 출력한다. 트리 노드가 특정한 방식으로 구성되어 있으면 순서대로 이동하여 오름차순으로 트리 노드를 방문한다. 

이를 코드로 구현해보자.
```swift
extension BinaryNode {
    public func traverseInOrder(visit: (Element) -> Void) {
        leftChild?.traverseInOrder(visit: visit)
        visit(value)
        rightChild?.traverseInOrder(visit: visit)
    }
}

print("Example of in-order traversal")
tree.traverseInOrder { print($0) }
/*
 Example of in-order traversal
 0
 1
 5
 7
 8
 9
 */
```

### 전위 순회 | Pre-order traversal
전위 순회는 항상 현재 노드를 첫 번째로 방문하고, 왼쪽 자식과 오른쪽 자식을 연속적으로 방문한다.

<img width="200" alt="스크린샷 2024-03-13 오후 7 12 06" src="https://github.com/Kim-leo/TIL/assets/77371366/74c32c1e-0c97-49b0-a545-47738a3e4255">

이를 바로 코드로 구현해보자.
```swift
public func traversePreOrder(visit: (Element) -> Void) {
    visit(value)
    leftChild?.traversePreOrder(visit: visit)
    rightChild?.traversePreOrder(visit: visit)
}

print("Example of pre-order traversal")
tree.traversePreOrder { print($0) }
/*
 Example of pre-order traversal
 7
 1
 0
 5
 9
 8
 */
```

### 후위 순회 | Post-order traversal
후위 순회는 왼쪽 자식과 오른쪽 자식을 연속적으로 방문하고 현재 노드를 방문한다.

<img width="259" alt="스크린샷 2024-03-13 오후 7 15 46" src="https://github.com/Kim-leo/TIL/assets/77371366/741f0e61-bbe1-4bc8-a5d9-28310ff78351">

다시 말해서, 어느 노드로부터 시작하던간에 자기 자신 노드보다 자식부터 방문한다.그래서 항상 루트 노드는 맨 마지막으로 방문한다.

이것도 코드로 만들어보자.
```swift
public func traversePostOrder(visit: (Element) -> Void) {
    leftChild?.traversePostOrder(visit: visit)
    rightChild?.traversePostOrder(visit: visit)
    visit(value)
}

print("Example of post-order traversal")
tree.traversePostOrder { print($0) }
/*
 Example of post-order traversal
 0
 5
 1
 8
 9
 7
 */
```

이 세 가지의 수노히 알고리즘은 시간 복잡도와 공간 복잡도가 모두 __O(n)__ 이다. 

중위 순회 알고리즘은 오름차순으로 노드를 방문한다. 이진 트리는 삽입 과정에서 이러한 규칙들을 준수함으로써 이를 보장할 수 있다. 

### 요점 정리
- 이진 트리는 가장 중요한 트리 자료구조 형태의 근본이 된다. __이진 탐색__ 트리와 AVL 트리는 삽입, 제거 동작에 제한이 있는 이진 트리이다.
- 중위 순회(in-order), 전위 순회(pre-order), 후위 순회(post-order) 방법은 이진 트리에만 중요한 것은 아니다. 어느 트리 구조에서 데이터를 처리할 때, 이 순회 방법을 자주 사용할 것이다.

