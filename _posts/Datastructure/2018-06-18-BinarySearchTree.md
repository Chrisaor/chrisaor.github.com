---
layout: post
title:  "[Tree] Binary Search Tree"
date:   2018-06-18 12:20:00 +0900
categories: datastructure
---

다음의 성격들을 가지는 노드 기반의 이진 트리 자료 구조이다.

- 노드의 왼쪽 하위 트리는 노드의 키보다 작은 키가 있는 노드만 포함된다.

- 노드의 오른쪽 하위 트리에는 노드의 키보다 큰 키가 있는 노드만 포함된다.

- 왼쪽 및 오른쪽 하위 트리는 각각 이진 탐색 트리여야한다. 중복된 노드가 없어야함.

![image](https://user-images.githubusercontent.com/33015649/41514962-1aefc6a0-72e6-11e8-84e8-c4fa69407c19.png)

위의 이진 탐색 트리의 속성은 검색, 최소 및 최대와 같은 작업을 빠르게 수행하 ㄹ수 있도록 키 순서를 제공한다. 순서가 없는 경우, 지정된 키를 탐색하기 위해 모든 키를 비교해야할 것이다.

**키 찾기**

이진 탐색 트리에서 주어진 키를 검색하려면 먼저 루트와 비교하고 키가 루트에 있으면 루트를 반환한다. 키가 루트의 키보다 크다면 오른쪽, 작으면 왼쪽 하위트리로 반복 검색한다.

```python
def search(root,key):
     
    # Base Cases: 루트가 비어있거나 루트의 값이 키와 같을 때 루트를 반환
    if root is None or root.val == key:
        return root
 
    # 키가 루트의 값보다 클 때 오른쪽으로 감
    if root.val < key:
        return search(root.right,key)
   
    # 키가 루트의 값보다 작을 때 왼쪽으로 감
    return search(root.left,key)
```

아래 트리에서 6찾기

1. 루트에서 시작
2. 들어간 원소와 루트를 비교한다. 만약에 루트보다 작으면 왼쪽으로 recurse하고 그렇지 않으면 오른쪽으로 recurser함
3. 원소를 찾으면 true를 리턴하고 아니면 false를 리턴함. 

![image](https://user-images.githubusercontent.com/33015649/41757385-cd453c34-761c-11e8-8521-0c724a13cc88.png)


**키 삽입하기**

새로운 키는 항상 리프에 삽입된다. 리프 노드를 도달할 때까지 루트에서부서 키를 검색한다. 리프 노드가 발견되면 새 노드가 리프 노드의 자식 노드로 추가된다.

```
         100                               100
        /   \        Insert 40            /    \
      20     500    --------->          20     500 
     /  \                              /  \  
    10   30                           10   30
                                              \   
                                              40
```

```python
class Node:
    def __init__(self,key):
        self.left = None
        self.right = None
        self.val = key
 
# A utility function to insert a new node with the given key
def insert(root,node):
    if root is None:
        root = node
    else:
        if root.val < node.val:
            if root.right is None:
                root.right = node
            else:
                insert(root.right, node)
        else:
            if root.left is None:
                root.left = node
            else:
                insert(root.left, node)
 
# A utility function to do inorder tree traversal
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val)
        inorder(root.right)
 
 
# Driver program to test the above functions
# Let us create the following BST
#      50
#    /      \
#   30     70
#   / \    / \
#  20 40  60 80
r = Node(50)
insert(r,Node(30))
insert(r,Node(20))
insert(r,Node(40))
insert(r,Node(70))
insert(r,Node(60))
insert(r,Node(80))
 
# Print inoder traversal of the BST
inorder(r)
```

**Output**:

```
20
30
40
50
60
70
80
```

**아래 그림에서 2를 넣는 과정**

![image](https://user-images.githubusercontent.com/33015649/41757620-21ae4a6c-761e-11e8-8cd1-fa91406ef357.png)


1. 루트에서 시작한다.
2. 삽입되는 키를 루트와 비교한다. 루트보다 작으면 왼쪽, 아니면 오른쪽으로 recurse한다.
3. 리프까지 도달하면 그 리프보다 작다면 왼쪽, 아니면 오른쪽에 삽입된다.

**시간 복잡도 :**

검색 및 삽입 연산의 최악의 시간 복잡도는 O(h)이다. h는 이진 검색 트리의 높이를 말함. 최악의 경우, 루트에서 가장 깊은 리프 노드로 이동해야 할 수도 있다. 검색 및 삽입 작업의 시간 복잡도는 O(n)이 될 수 있다.



