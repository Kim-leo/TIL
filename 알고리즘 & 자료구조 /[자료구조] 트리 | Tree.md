# 트리 | Tree 

<img width="471" alt="스크린샷 2024-03-11 오전 11 48 35" src="https://github.com/Kim-leo/TIL/assets/77371366/daa2fd20-7e56-4bfb-8a2d-56849451d552">

트리는 중요한 비선형 리스트(Non-linear list) 자료구조 형태 중 하나이다. 
계층 관계를 표현하거나, 정렬된 데이터를 관리하거나, <code>lookup</code> 연산을 빠르게 하는 등에 사용된다.

트리에는 많은 종류가 있으며 종류마다 형태나 크기가 다양하다. 

### 용어 정리

__Node__ 

연결리스트와 마찬가지로 트리는 __노드__ 로 이루어져 있다. 

<img width="188" alt="스크린샷 2024-03-11 오전 11 54 07" src="https://github.com/Kim-leo/TIL/assets/77371366/770321eb-b613-41a0-8f48-be33f6710d02">

노드는 데이터를 담을 수 있으며 __자식(children)__ 을 추적할 수 있다. 

__Parent & Child__

트리는 실제 나무가 위에서부터 아래로 가지를 뻗어 내려가듯이, 맨 상단 부분에서부터 아래로 가지를 뻗어나가는 구조로 되어있다.

맨 윗 상단의 노드를 제외한 모든 노드는 자신 위에 하나의 노드를 가지는데, 그 상단 노드를 __부모(parent)__ 라고 한다. 부모 노드는 아래로 뻗어가며 __자식(child__ 노드를 여러 개 가질 수 있다. 트리에서는 모든 자식은 하나의 부모를 가진다.

<img width="206" alt="스크린샷 2024-03-11 오전 11 58 49" src="https://github.com/Kim-leo/TIL/assets/77371366/298a1dcc-c94e-4623-ad97-f35d96888ce4">

__Root__

트리의 가장 맨 윗 상단의 노드를 __루트(root)__ 라고 한다. 루트 위의 부모는 없다. 

<img width="277" alt="스크린샷 2024-03-11 오전 11 59 40" src="https://github.com/Kim-leo/TIL/assets/77371366/247e0598-dc9c-4dba-9aaf-69cd16713f0a">

__Leaf__ 

리프는 자식이 더 이상 없는 맨 아래 노드를 말한다.

<img width="321" alt="스크린샷 2024-03-11 오후 12 00 17" src="https://github.com/Kim-leo/TIL/assets/77371366/1c30e3d6-78d0-4784-9afd-284cbf09e9d6">

### 코드로 트리 구현하기
트리 구조를 직접 코드로 구현하기 위해 아래와 같은 코드를 입력한다.
```swift
public class TreeNode<T> {
    public var value: T
    public var children: [TreeNode] = []
    
    public init(_ value: T) {
        self.value = value
    }
}
```

각 노드는 <code>value</code> 를 담당하며 배열을 사용하여 모든 자식에 대한 참조를 가진다.

> class 타입으로 <code>TreeNode</code>를 생성하면 <code>value</code>의 의미를 잃어버릴 수 있다. 반면에, 이는 노드에 대한 참조를 만들고, 이를 나중에 사용할 예정이다.

<code>TreeNode class</code> 안에 아래와 같은 메소드를 추가한다.
```swift
public func add(_ child: TreeNode) {
    children.append(child)
}
```

위 메소드는 한 노드에 자식 노드를 추가하는 기능을 수행한다.

이제, 코드 맨 아래에 아래와 같이 직접 트리를 선언하고 부모-자식 관계를 만든다.

```swift
print("Example of creating a tree")
let beverages = TreeNode("Beverages")

let hot = TreeNode("Hot")
let cold = TreeNode("Cold")

beverages.add(hot)
beverages.add(cold)
```

계층 구조는 트리 구조의 자연스러운 후보이므로 여기서는 세 개의 서로 다른 노드를 정의하고 논리적 계층 구조로 구성했다. 이 배열은 다음 구조에 해당한다.

<img width="172" alt="스크린샷 2024-03-11 오후 12 10 26" src="https://github.com/Kim-leo/TIL/assets/77371366/97b7e503-22e1-4217-98c1-69b91e433ee8">

### 트리 순회 알고리즘 | Traversal algorithms

배열이나 연결리스트 등의 선형 구조를 반복하는 것은 간단하다. 선형 구조는 시작과 끝이 명확하다.

<img width="387" alt="스크린샷 2024-03-11 오후 12 14 29" src="https://github.com/Kim-leo/TIL/assets/77371366/3fcb37ec-9d8e-4574-8df1-17b3854197de">

트리에서 반복하는 것은 더 복잡하다.

<img width="375" alt="스크린샷 2024-03-11 오후 12 14 59" src="https://github.com/Kim-leo/TIL/assets/77371366/8df66aa6-8e2b-4896-9ed1-6ae63e0cbece">

어느 노드를 우선적으로 탐색해야 할까? 다양한 트리와 다양한 문제에 대한 수 가지 순회 전략이 존재하는데 바로 다음에는 __깊이 우선 탐색(depth-first traversal)__ 을 알아볼 것이다. 깊이 우선 탐색은 루트에서 시작하여 백트래킹 하기 전에 가장 깊이 있는 노드를 방문하는 기법이다.

### 깊이 우선 탐색 | Depth-first traversal

<code>TreeNode</code>에 대한 <code>extension</code>를 추가한다.

```swift
extension TreeNode {
    public func forEachDepthFirst(visit: (TreeNode) -> Void) {
        visit(self)
        children.forEach {
            $0.forEachDepthFirst(visit: visit)
        }
    }
}
```

위 코드는 재귀를 사용하여 다음 노드를 처리한다. 구현을 재귀적으로 하지 않으려면 자신의 스택을 사용할 수 있다.

코드 맨 아래에 아래와 같이 작성한다.
```swift
func makeBeverageTree() -> TreeNode<String> {
    let tree = TreeNode("Beverages")
    let hot = TreeNode("hot")
    let cold = TreeNode("cold")
    let tea = TreeNode("tea")
    let coffee = TreeNode("coffee")
    let chocolate = TreeNode("cocoa")
    let blackTea = TreeNode("black")
    let greenTea = TreeNode("green")
    let chaiTea = TreeNode("chai")
    let soda = TreeNode("soda")
    let milk = TreeNode("milk")
    let gingerAle = TreeNode("ginger ale")
    let bitterLemon = TreeNode("bitter lemon")
    tree.add(hot)
    tree.add(cold)
    hot.add(tea)
    hot.add(coffee)
    hot.add(chocolate)
    cold.add(soda)
    cold.add(milk)
    tea.add(blackTea)
    tea.add(greenTea)
    tea.add(chaiTea)
    soda.add(gingerAle)
    soda.add(bitterLemon)
    return tree
}

print("Example of depth-first traversal")
let tree = makeBeverageTree()
tree.forEachDepthFirst { print($0.value) }
/* 결과값
Example of depth-first traversal
Beverages
hot
tea
black
green
chai
coffee
cocoa
cold
soda
ginger ale
bitter lemon
milk
*/
```

### 레벨 순회 | Level-order traversal
레벨 순회 방법은 노드의 깊이를 기반으로 트리의 각 노드를 방문하는 방법이다.
```swift
extension TreeNode {
    public func forEachLevelOrder(visit: (TreeNode) -> Void) {
        visit(self)
        var queue = Queue<TreeNode>()
        children.forEach { queue.enqueue($0) }
        while let node = queue.dequeue() {
            visit(node)
            node.children.forEach { queue.enqueue($0)}
        }
    }
}
```
<code>forEachLevelOrder</code>는 층 별 순서대로 각각의 노드를 방문한다.

<img width="312" alt="스크린샷 2024-03-11 오후 12 34 25" src="https://github.com/Kim-leo/TIL/assets/77371366/ad2537c6-709f-4263-a4d8-6014f3fd6b44">

올바른 수준의 순서로 노드를 방문하기 위해 (스택이 아닌) 큐를 사용했다. 간단한 재귀(스택을 암시적으로 사용)는 작동하지 않았을 것이다.

```swift
print("Example of level-order traversal")
let tree = makeBeverageTree()
tree.forEachLevelOrder { print($0.value) }
/* 결과값
Example of level-order traversal
Beverages
hot
cold
tea
coffee
cocoa
soda
milk
black
green
chai
ginger ale
bitter lemon
*/
```

### 탐색 | Search

모든 노드를 순회하는 메소드를 이미 만들었으니, 탐색 알고리즘을 위한 코드도 금방 구현할 수 있다. 

```swift
extension TreeNode where T: Equatable {
    public func search(_ value: T) -> TreeNode? {
        var result: TreeNode?
        forEachLevelOrder { node in
            if node.value == value {
                result = node
            }
        }    
        return result
    }
}

print("Example of searching for a node")
if let searchResult1 = tree.search("ginger ale") {
    print("Found node: \(searchResult1.value)")
}
if let searchResult2 = tree.search("WKD Blue") {
    print(searchResult2.value)
} else {
    print("Couldn't find WKD Blue")
}
/* 결과값
Found node: ginger ale
Couldn't find WKD Blue
*/
```

여기서는 __레벨 순서 순회 알고리즘__ 을 사용했다. 이 코드는 모든 노드를 방문하기 때문에 여러 개의 일치가 있으면 마지막 일치에 대한 값을 반환한다. 이러한 불안정성은 어떤 순회를 사용하느냐에 따라 다른 객체를 돌려받게 된다는 것을 의미한다.

### 요점 정리
- 트리는 연결리스트과 유사하지만 연결리스트는 하나의 노드에 하나의 자식 노드를 가지지만, 트리는 여러 개의 자식 노드를 가질 수 있는 차이가 있다.

- 모든 트리 노드는 루트 노드를 제외하고 하나의 부모 노드를 가진다.

- 루트 노드는 부모 노드가 없고, 리프 노드는 자식 노드가 없다.

- 깊이 우선 탐색, 레벨 순회 탐색 등의 탐색 방법은 일반적인 트리 구조에서 명확하지 않고, 다른 종류의 트리에서 작동한다. 트리 구조마다 연산 방법이 살짝씩 다르다.
