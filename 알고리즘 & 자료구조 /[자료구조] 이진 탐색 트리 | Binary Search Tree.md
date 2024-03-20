# 이진 탐색 트리 | Binary Search Tree

이진 탐색 트리(Binary Search Tree, BST)는 <code>fast lookup, insert, removal</code> 연산에 특화된 자료 구조이다. 한 쪽을 선택하면 다른 쪽의 모든 가능성이 상실되어 문제가 절반으로 줄어드는 다음 의사 결정 트리를 살펴본다.

![스크린샷 2024-03-14 오후 2 32 20](https://github.com/Kim-leo/TIL/assets/77371366/794f46d2-e56e-4a17-ac3b-29ff599dd488)

일단 결정을 내리고 분기를 선택하면 뒤를 돌아갈 수 없다. 리프 노드에서 최종 결정을 내릴 때까지 계속 진행한다. 

이진 트리를 사용하면 같은 일을 할 수 있다. 특히 이진 탐색 트리는 이전 장에서 보았던 이진 트리에 두 가지 규칙을 부여한다.

- 왼쪽 자식의 값은 부모의 값보다 작아야 한다.
- 따라서, 오른쪽 자식의 값은 부모 노드보다 같거나 커야 한다.

이진 탐색 트리는 이 속성을 사용하여 불필요한 검사를 수행하는 것을 방지한다. 따라서 검색, 삽입 및 제거는 평균적으로 __O(log n)__ 의 시간 복잡도를 가지며, 이는 배열 및 연결리스트와 같은 선형 데이터 구조보다 상당히 빠르다.

# 사례로 보기: 배열 vs 이진 탐색 트리(BST)

이진 탐색 트리의 좋은 특징을 알기 위해 몇가지 일반적인 연산과 배열과 성능을 비교한다. 아래 배열과 이진 탐색 트리가 있다.

![스크린샷 2024-03-14 오후 2 38 44](https://github.com/Kim-leo/TIL/assets/77371366/7c4749a1-0809-449d-84f1-70fc97705ae3)

### 조회 | Lookup
정렬되지 않은 배열에서 값을 조회하는 방법은 하나밖에 없다. 첫 번째부터 값을 하나씩 다 살펴보는 것이다.
그래서 <code>array.contains(_:)</code> 의 시간 복잡도가 __O(n)__ 이다.

![스크린샷 2024-03-14 오후 2 40 50](https://github.com/Kim-leo/TIL/assets/77371366/c9807f0b-ae28-4381-993a-fd81015b9ca4)

하지만 이진 탐색 트리는 다르게 조회한다.

![스크린샷 2024-03-14 오후 2 41 10](https://github.com/Kim-leo/TIL/assets/77371366/ed83e05c-8960-4319-b711-69ebed8df8af)

검색 알고리즘이 이진 탐색 트리의 노드를 방문할 때마다 아래 두 가지 가정을 안전하게 수행한다.

- 만약 조회하고자 하는 값이 현재 값보다 작다면, 왼쪽 하위 트리로 이동한다.
- 조회하고자 하는 값이 현재 값보다 크다면, 오른쪽 하위 트리로 이동한다.

이진 탐색 트리의 규칙을 따르는 상태에서, 불필요한 조회를 피하며 의사결정을 할 때마다 탐색 공간을 반으로 줄일 수 있다. 그래서 <code>lookup</code> 연산이 __O(log n)__ 의 시간 복잡도를 가지는 것이다.

### 삽입 | Insertion
삽입 연산의 이점은 배열과 이진 탐색 트리가 비슷하다. 0 값을 아래 배열 또는 이진 탐색 트리에 삽입한다고 가정한다.

![스크린샷 2024-03-14 오후 2 45 57](https://github.com/Kim-leo/TIL/assets/77371366/caf39a40-d0d2-4666-b6fe-91f263f1f754)

배열에 값을 삽입하는 것은 기존 행에 끼어드는 것과 비슷하다. 선택한 지점 뒤에 줄에 있는 모든 사람은 뒤로 뒤로 밀어서 공간을 만들어야 한다.

위 배열의 그림에서와 같이 0은 배열의 맨 앞에 삽입되는데 그럼으로써 기존의 모든 값들이 한칸씩 뒤로 밀리게 된다. 배열에 값을 삽입하는 연산은 __O(n)__ 의 시간 복잡도를 가진다.

이진 탐색 트리에 값을 삽입하는 것은 보다 더 간단하다.

![스크린샷 2024-03-14 오후 2 49 25](https://github.com/Kim-leo/TIL/assets/77371366/c4b42768-edd6-4e16-be6b-6f79449c45b6)

이진 탐색 트리의 규칙을 따르는 상태에서, 삽입을 위한 장소를 찾기 위한 단 세 번의 이동만 하면 된다. 모든 기존의 값들을 섞을 필요도 없다.
이진 탐색 트리에 값을 삽입하는 것은 __O(log n)__ 의 시간 복잡도를 가진다.

### 제거 | Removal
삽입과 비슷하게 배열에서 값을 제거하는 것 또한 기존의 값들의 위치를 바꿔야 한다.

![스크린샷 2024-03-14 오후 2 51 44](https://github.com/Kim-leo/TIL/assets/77371366/9ff47347-8e5a-4553-bfd1-5025e4b634fa)

이는 줄을 서는 비유와 같다. 줄 가운데를 떠나면 뒤에 있는 모든 사람들이 앞으로 몸을 섞어야 빈 공간을 차지할 수 있다.

이번에는 이진 탐색 트리에서 값을 제거하는 방법이다.

![스크린샷 2024-03-14 오후 3 01 18](https://github.com/Kim-leo/TIL/assets/77371366/84ed9b06-e396-41a5-bf1e-7f6278f8e358)

제거할 노드에 자식이 있는 경우 조금 까다롭지만 이는 나중에 자세히 살펴볼 것이다. 이러한 문제가 있더라도 이진 탐색 트리에서 요소를 제거하는 것은 여전히 __O(log n)__ 의 시간 복잡도를 가진다.

이진 탐색 트리는 추가, 제거 및 조회 작업의 단계 수를 크게 줄인다. 이진 탐색 트리를 사용하는 것의 이점을 이해했으니, 실제 구현으로 넘어갈 수 있다.

### 코드로 구현하기
이진 탐색 트리를 직접 코드로 만들어 본다.
```swift
public struct BinarySearchTree<Elemenet: Comparable> {
    public private(set) var root: BinaryNode<Elemenet>?
    
    public init() {}
}

extension BinarySearchTree: CustomStringConvertible {
    public var description: String {
        guard let root = root else { return "Empty tree" }
        return String(describing: root)
    }
}
```

### 삽입 연산 | Inserting elements
이진 탐색 트리(BST)의 규칙에 따라 왼쪽 자식 노드는 현재 노드보다 작은 값을 포함해야 한다. 오른쪽 자식 노드는 현재 노드보다 크거나 같은 값을 포함해야한다. 이러한 규칙을 따르는 <code>insert</code> 연산을 만들어 본다.
```swift
extension BinarySearchTree {
    public mutating func insert(_ value: Elemenet) {
        root = insert(from: root, value: value)
    }
    
    private func insert(from node: BinaryNode<Elemenet>?, value: Elemenet) -> BinaryNode<Elemenet> {
        // 1번
        guard let node = node else {
            return BinaryNode(value: value)
        }
        // 2번
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        // 3번
        return node
    }
}
```

첫 번째 <code>insert</code> 연산은 사용자에게 보여지고, 두 번째 <code>insert</code> 연산은 private helper 메서드에 사용된다.

- 1번: 위 메서드는 재귀적 방법을 사용했으므로 재귀를 종료하려면 기본 케이스가 필요하다. 만약 현재 노드기 <code>nil</code>이라면 삽입 포인트를 찾아서 새로운 <code>BinaryNode</code>를 반환한다.
- 2번: <code>Element</code> 타입이 <code>Comparable</code>하기 때문에, 비교를 수행할 수 있다. <code>if</code>문은 다음 <code>insert</code>연산이 어디로 순회되어야 하는지 제어한다. 만약 새 값이 현재 값보다 작다면 <code>insert</code>은 왼쪽 자식으로 진행되고, 새 값이 현재 값보다 크거나 같다면 <code>insert</code>을 오른쪽 자식으로 진행하면 된다.
- 3번: 현재 노드를 반환한다. <code>insert</code> 연산이 <code>node (if it was nil)</code>을 만들거나, <code>node(if it was not nil)</code>을 반환하기 때문에 <code>node = insert(from: node, value: value)</code> 형식에 할당이 가능하다.

이제 실제로 값을 삽입해보자.
```swift
print("Example of building a BST")
var bst = BinarySearchTree<Int>()
for i in 0..<5 {
    bst.insert(i)
}
print(bst)
/* 결과값
Example of building a BST
   ┌──4 
  ┌──3
  │ └──nil 
 ┌──2
 │ └──nil 
┌──1
│ └──nil 
0
└──nil 

*/
```
위의 트리는 약간 불균형이지만 규칙을 따르긴 한다. 하지만, 이 트리 배치는 바람직하지 못한 결과를 초래한다. 트리를 작업할 때 항상 균형있는 포맷으로 만들어야 한다.

![스크린샷 2024-03-15 오후 1 11 40](https://github.com/Kim-leo/TIL/assets/77371366/0d08686f-643c-4f5a-8df0-51a492852fcf)

불균형적인 트리는 성능에 영향을 끼치는데, 5라는 값을 새로 삽입한다고 하면, __O(n)__ 시간 복잡도를 가지게 된다.

![스크린샷 2024-03-15 오후 1 12 31](https://github.com/Kim-leo/TIL/assets/77371366/267337c3-d602-4ebd-90e8-baad658af88c)

균형 잡힌 구조를 유지하기 위해 다른 기술을 사용하는 셀프 밸런싱 트리로 알려진 구조를 만들 수 있지만, 이러한 세부 사항은 "AVL 트리"에서 알아본다. 지금은 불균형이 발생하지 않도록 약간의 주의를 기울여 샘플 트리를 만들 것이다.
```swift
var exampleTree: BinarySearchTree<Int> {
    var bst = BinarySearchTree<Int>()
    bst.insert(3)
    bst.insert(1)
    bst.insert(4)
    bst.insert(0)
    bst.insert(2)
    bst.insert(5)
    return bst
}

print("Example of building a BST")
print(exampleTree)
/* 결과값
Example of building a BST
 ┌──5 
┌──4
│ └──nil 
3
│ ┌──2 
└──1
 └──0 
*/
```
### 탐색 연산 | Finding elements
이진 탐색 트리에서 요소를 찾으려면 해당 노드를 방문해야 한다. 이전 장에서 배운 기존의 순회 메서드를 사용하면 비교적 간단한 구현을 할 수 있다.
```swift
extension BinarySearchTree {
    public func contains(_ value: Element) -> Bool {
        guard let root = root else {
            return false
        }
        var found = false
        root.traverseInOrder {
            if $0 == value {
                found = true
            }
        }
        return found
    }
}

print("example of finding a node")
if exampleTree.contains(5) {
    print("Found 5!")
} else {
    print("Couldn’t find 5")
}
/* 결과값
example of finding a node
Found 5!
*/
```

중위 순회(In-roder traversal) 알고리즘은 시간 복잡도가 __O(n)__ 이므로 <code>contains</code> 연산은 정렬되지 않은 배열을 통한 전체 검색에 대하여 같은 시간 복잡도를 가진다. 하지만, 이래와 같이 최적화를 할 수 있다.

이진 탐색 트리의 규칙을 지키면서 불필요햔 비교 연산을 피할 수 있다.

```swift
public func contains(_ value: Element) -> Bool {
    // 1 번
    var current = root
    // 2 번
    while let node = current {
        // 3 번
        if node.value == value {
            return true
        }
        // 4 번
        if value < node.value {
            current = node.leftChild
        } else {
            current = node.rightChild
        }
    }
    return false
}
```
- 1번: <code>current</code> 변수를 루트 노드로 설정.
- 2번: <code>current</code> 가 <code>nil</code> 이 아닐 때까지, 현재 노드의 값을 확인.
- 3번: 그 값을 찾았다면, <code>true</code> 를 반환.
- 4번: 그렇지 않다면, 왼쪽 또는 오른쪽 자식으로 갈지 결정.

바로 위의 <code>contains</code> 연산은 군형적 이진 탐색 트리에서 __O(log n)__ 의 시간 복잡도를 가진다.

### 제거 연산 | Removing elements
제거 연산은 살짝 더 까다롭다. 케이스 별로 나뉘는데 아래 케이스들을 살펴보자.

### Case 1: 리프 노드
리프 노드를 제거하는 것은 직관적이다. 리프 노드를 떼내면 끝이다.

![스크린샷 2024-03-20 오후 4 32 17](https://github.com/Kim-leo/TIL/assets/77371366/5b0285f1-40c1-4928-89dc-44b0beec41f5)

하지만, 리프 노드가 아닌 것을 제거하려면 몇 가지 단계가 필요하다.

### Case 2: 하나의 자식만 가진 노드
하나의 자식만 가진 노드를 제거할 때에는 트리의 나머지 부분에 그 하나의 자식을 재연결 해야 한다.

![스크린샷 2024-03-20 오후 4 33 32](https://github.com/Kim-leo/TIL/assets/77371366/2cbe8dd8-9546-43b7-bcf3-58a68b4f8ab8)

### Case 3: 두 개의 자식을 가진 노드
두 개의 자식을 가진 노드들은 더 복잡한데, 아래 그림처럼 25 값을 제거하는 것을 예시로 보자.

![스크린샷 2024-03-20 오후 4 34 47](https://github.com/Kim-leo/TIL/assets/77371366/61f3a0d5-92f8-4e5e-85cb-7cca59e7f16c)

단순히 그 값을 제거만 한다면 아래와 같이 딜레마에 빠진다.

![스크린샷 2024-03-20 오후 4 35 19](https://github.com/Kim-leo/TIL/assets/77371366/2baa7a55-6e27-4375-97be-8d49083af993)

25 값의 자식들인 12와 37을 재연결해야 하는데, 부모 노드에게는 하나의 자식만 들어갈 수 있는 공간 밖에 없다. 이를 해결하기 위해서 <code>swap</code> 을 수행함으로써 해결한다.

두 개의 자식을 가진 노드를 제거할 때에는, 오른쪽 자식 서브트리의 가장 작은 값을 가져와 대체한다. 이진 탐색 트리 규칙에 근거하여, 이는 오른쪽 자식 서브 트리의 최좌측의 값이다.

![스크린샷 2024-03-20 오후 4 38 15](https://github.com/Kim-leo/TIL/assets/77371366/7ae991a0-b666-4bca-9455-511074446fba)

이를 통해 유효한 이진 탐색 트리가 되는지 확인해야 한다. 새 노드가 오른쪽 서브 트리에서 가장 작은 값이므로, 오른쪽 서브 트리는 여전히 새 노드보다 크거나 같을 것이다. 그리고, 새 노드가 오른쪽 서브 트리에서 나왔으므로 왼쪽 서브 트리의 모든 노드는 새 노드보다 작을 것이다.

<code>swap</code> 을 한 뒤, 그 값을 제거한다.

![스크린샷 2024-03-20 오후 4 40 29](https://github.com/Kim-leo/TIL/assets/77371366/e488ebbe-38bc-414b-8dae-0a1c07ea3b08)

이를 코드로 구현해보자.
```swift
private extension BinaryNode {
    var min: BinaryNode {
        leftChild?.min ?? self
    }
}
extension BinarySearchTree {
    public mutating func remove(_ value: Element) {
        root = remove(node: root, value: value)
    }
    private func remove(node: BinaryNode<Element>?, value: Element) -> BinaryNode<Element>? {
        guard let node = node else {
            return nil
        }
        if value == node.value {
            // 1 번
            if node.leftChild == nil && node.rightChild == nil {
            return nil
            }
            // 2 번
            if node.leftChild == nil {
              return node.rightChild
            }
            // 3 번
            if node.rightChild == nil {
              return node.leftChild
            }
            // 4 번
            node.value = node.rightChild!.min.value
            node.rightChild = remove(node: node.rightChild, value:
            node.value)
        } else if value < node.value {
              node.leftChild = remove(node: node.leftChild, value: value)
        } else {
              node.rightChild = remove(node: node.rightChild, value: value)
        }
        return node
    }
}
```

이 방법은 값을 삽입할 때와 동일한 재귀적 설정을 <code>private helpter method</code>로 사용한다. 또한 <code>BinaryNode</code>에 재귀적 최소 속성을 추가하여 하위 트리의 최소 노드를 찾는다. 다른 제거 케이스들은 <code>if value == node.value</code> 절에서 처리된다.

- 1번: 노드가 리프 노드이면, <code>nil</code>을 반환한다. 그러면 현재 노드를 제거하게 된다.
- 2번: 노드가 왼쪽 자식이 없으면 오른쪽 서브 트리와 재연결하기 위해서 <code>node.rightChild</code>를 반환한다.
- 3번: 노드가 오른쪽 자식이 없으면 왼쪽 서브 트리와 재연결하기 위해서 <code>node.leftChild</code>를 반환한다.
- 4번: 제거될 노드가 왼쪽, 오른쪽 자식이 둘 다 있는 경우이다. 오른쪽 서브 트리에서 가장 작은 값으로 변경하고 그 변경된 원래 노드를 제거한다.

직접 노드를 제거해보자.
```swift
print(example of removing a node")
var tree = exampleTree
print("Tree before removal: ")
print(tree)
tree.remove(3)
print("Tree after removal: ")
print(tree)
/* 결과값
---Example of: removing a node---
Tree before removal:
┌──5 ┌──4
│ └──nil
3
│ ┌──2
└──1
└──0
Tree after removing root:
┌──5
4
│ ┌──2
└──1 └──0
*/
```

### 요점 정리
- 이진 탐색 트리는 정렬된 데이터를 담는데 아주 좋다.
- 이진 탐색 트리의 요소들은 비교할 수 있는 형태이어야 한다. 일반적인 제약 조건을 사용하거나 비교를 수행하기 위해서 <code>closure</code>를 제공하여 이를 수행할 수 있다.
- 삽입, 제거, 탐색 의 시간 복잡도는 __O(log n)__ 이다.
- 트리가 균형이 맞지 않고 한 쪽으로 쏠린다면 __O(n)__ 으로 성능이 저하될 것이다.
