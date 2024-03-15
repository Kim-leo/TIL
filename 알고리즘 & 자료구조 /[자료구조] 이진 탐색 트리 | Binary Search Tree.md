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
