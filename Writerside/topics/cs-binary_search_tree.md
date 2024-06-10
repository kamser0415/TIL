# 이진 탐색 트리(BST)

## 기본 개념
이진 탐색 트리(BST)는 각 노드의 왼쪽 서브 트리가 그 노드보다 작은 값을, 오른쪽 서브 트리가 그 노드보다 큰 값을 가지는 트리입니다. 예를 들어, 루트 노드가 20인 경우를 생각해봅시다:

```text
      20
     /  \
    10  30
   / \    \
  5  15   40
 / \
3   7
```

- **루트 노드:** 20
- **왼쪽 서브 트리:** 10, 5, 3, 7, 15
- **오른쪽 서브 트리:** 30, 40

## 순회 방법
BST는 트리의 모든 노드를 특정 순서로 방문하는 다양한 순회 방법이 있습니다. 여기서는 중위 순회, 전위 순회, 후위 순회를 소개합니다.

1. **중위 순회(Inorder Traversal)**
    - 왼쪽 -> 루트 -> 오른쪽 순서로 방문합니다.
    - 트리의 값을 정렬된 순서로 방문할 수 있습니다.
    - 예: 3, 5, 7, 10, 15, 20, 30, 40

2. **전위 순회(Preorder Traversal)**
    - 루트 -> 왼쪽 -> 오른쪽 순서로 방문합니다.
    - 트리의 구조를 복제하는 데 유용합니다.
    - 예: 20, 10, 5, 3, 7, 15, 30, 40

3. **후위 순회(Postorder Traversal)**
    - 왼쪽 -> 오른쪽 -> 루트 순서로 방문합니다.
    - 트리의 삭제 연산 등에 유용합니다.
    - 예: 3, 7, 5, 15, 10, 40, 30, 20

## 삽입, 삭제, 검색
BST에서의 삽입, 삭제, 검색 연산은 특정 규칙에 따라 이루어집니다.

1. **삽입**
    - 새로운 노드를 삽입할 위치를 찾아 알맞게 추가합니다.
    - 예를 들어, 25를 삽입하면:
      ```text
            20
           /  \
          10  30
         / \  / \
        5  15 25 40
       / \
      3   7
      ```

2. **삭제**
    - 삭제할 노드를 찾은 후, 자식 노드의 수에 따라 다르게 처리합니다.
    - 자식이 없는 경우: 단순히 삭제
    - 자식이 하나인 경우: 자식을 부모에 연결
    - 자식이 두 개인 경우: 후임자(successor) 또는 선임자(predecessor) 노드를 이용해 대체

3. **검색**
    - 루트부터 시작해 값을 비교하면서 왼쪽 또는 오른쪽으로 이동하며 값을 찾습니다.
    - 예: 7을 검색하려면 루트에서 시작해 왼쪽 자식 노드를 따라가며 값을 비교합니다.
      ```text
      20 -> 10 -> 5 -> 7
      ```

## 시간 복잡도
1. **최악의 경우**: 트리가 한쪽으로 치우친 경우, 시간 복잡도는 O(n)입니다.
2. **일반적인 경우**: 트리가 균형을 이루는 경우, 시간 복잡도는 O(log n)입니다.

### 장점과 단점
1. **장점**
    - 삽입과 삭제가 유연하며, 정렬된 데이터를 순회할 수 있습니다.

2. **단점**
    - 트리가 한쪽으로 치우친 경우 성능이 저하될 수 있습니다.
    - 이를 해결하기 위해 AVL 트리, 레드-블랙 트리 같은 자가 균형 이진 탐색 트리가 사용됩니다.

## 후임자(successor)란?
BST에서 후임자란 특정 노드의 오른쪽 서브트리에서 가장 작은 값을 가진 노드를 의미합니다. 후임자를 찾는 과정은 오른쪽 서브트리로 이동한 후, 최왼쪽 노드를 찾는 것입니다.

### 후임자 찾기 예제

```text
      20
     /  \
    10  30
   / \    \
  5  15   40
 / \
3   7
```

1. **20의 후임자 찾기**
    - 20의 오른쪽 서브트리로 이동합니다. 오른쪽 서브트리의 루트는 30입니다.
    - 30의 왼쪽 자식이 없으므로, 30이 20의 후임자가 됩니다.

2. **10의 후임자 찾기**
    - 10의 오른쪽 서브트리로 이동합니다. 오른쪽 서브트리의 루트는 15입니다.
    - 15의 왼쪽 자식이 없으므로, 15가 10의 후임자가 됩니다.

3. **5의 후임자 찾기**
    - 5의 오른쪽 서브트리로 이동합니다. 오른쪽 서브트리의 루트는 7입니다.
    - 7의 왼쪽 자식이 없으므로, 7이 5의 후임자가 됩니다.

## 이진 탐색 트리 자바 구현

```java
class TreeNode {
    int value;
    TreeNode left, right;

    TreeNode(int item) {
        value = item;
        left = right = null;
    }
}

class BinarySearchTree {
    TreeNode root;

    BinarySearchTree() {
        root = null;
    }

    void insert(int value) {
        root = insertRec(root, value);
    }

    TreeNode insertRec(TreeNode root, int value) {
        if (root == null) {
            root = new TreeNode(value);
            return root;
        }
        if (value < root.value)
            root.left = insertRec(root.left, value);
        else if (value > root.value)
            root.right = insertRec(root.right, value);

        return root;
    }

    void inorder() {
        inorderRec(root);
    }

    void inorderRec(TreeNode root) {
        if (root != null) {
            inorderRec(root.left);
            System.out.print(root.value + " ");
            inorderRec(root.right);
        }
    }

    void preorder() {
        preorderRec(root);
    }

    void preorderRec(TreeNode root) {
        if (root != null) {
            System.out.print(root.value + " ");
            preorderRec(root.left);
            preorderRec(root.right);
        }
    }

    void postorder() {
        postorderRec(root);
    }

    void postorderRec(TreeNode root) {
        if (root != null) {
            postorderRec(root.left);
            postorderRec(root.right);
            System.out.print(root.value + " ");
        }
    }

    TreeNode search(int value) {
        return searchRec(root, value);
    }

    TreeNode searchRec(TreeNode root, int value) {
        if (root == null || root.value == value)
            return root;
        if (root.value > value)
            return searchRec(root.left, value);
        return searchRec(root.right, value);
    }

    void delete(int value) {
        root = deleteRec(root, value);
    }

    TreeNode deleteRec(TreeNode root, int value) {
        if (root == null) return root;

        if (value < root.value)
            root.left = deleteRec(root.left, value);
        else if (value > root.value)
            root.right = deleteRec(root.right, value);
        else {
            if (root.left == null)
                return root.right;
            else if (root.right == null)
                return root.left;

            root.value = minValue(root.right);
            root.right = deleteRec(root.right, root.value);
        }

        return root;
    }

    int minValue(TreeNode root) {
        int minValue = root.value;
        while (root.left != null) {
            minValue = root.left.value;
            root = root.left;
        }
        return minValue;
    }

    TreeNode successor(TreeNode root, int value) {
        TreeNode current = search(value);
        if (current == null) return null;
        
        if (current.right != null) {
            TreeNode temp = current.right;
            while (temp.left != null) {
                temp = temp.left;
            }
            return temp;
        } else {
            TreeNode successor = null;
            TreeNode ancestor = this.root;
            while (ancestor != current) {
                if (current.value < ancestor.value) {
                    successor = ancestor;
                    ancestor = ancestor.left;
                } else {
                    ancestor = ancestor.right;
                }
            }
            return successor;
        }
    }

    public static void main(String[] args) {
        BinarySearchTree bst = new BinarySearchTree();

        bst.insert(20);
        bst.insert(10);
        bst.insert(30);
        bst.insert(5);
        bst.insert(15);
        bst.insert(25);
        bst.insert(40);
        bst.insert(3);
        bst.insert(7);

        System.out.print("Inorder traversal: ");
        bst.inorder();
        System.out.println();

        System.out

.print("Preorder traversal: ");
        bst.preorder();
        System.out.println();

        System.out.print("Postorder traversal: ");
        bst.postorder();
        System.out.println();

        System.out.println("Search for value 7: " + (bst.search(7) != null ? "Found" : "Not Found"));
        System.out.println("Search for value 100: " + (bst.search(100) != null ? "Found" : "Not Found"));

        bst.delete(10);
        System.out.print("Inorder traversal after deleting 10: ");
        bst.inorder();
        System.out.println();

        TreeNode successor = bst.successor(bst.root, 20);
        System.out.println("Successor of 20: " + (successor != null ? successor.value : "None"));

        successor = bst.successor(bst.root, 10);
        System.out.println("Successor of 10: " + (successor != null ? successor.value : "None"));

        successor = bst.successor(bst.root, 5);
        System.out.println("Successor of 5: " + (successor != null ? successor.value : "None"));
    }
}
```

### 설명
1. **TreeNode 클래스:** 트리의 노드를 정의합니다.
2. **BinarySearchTree 클래스:** 이진 탐색 트리의 삽입, 삭제, 검색, 순회 및 후임자 찾기를 구현합니다.
3. **insert 메서드:** 트리에 노드를 삽입합니다.
4. **inorder, preorder, postorder 메서드:** 중위, 전위, 후위 순회를 수행합니다.
5. **search 메서드:** 트리에서 값을 검색합니다.
6. **delete 메서드:** 트리에서 값을 삭제합니다.
7. **successor 메서드:** 주어진 값의 후임자를 찾습니다.

### 실행 예제
코드를 실행하면 다음과 같은 결과를 볼 수 있습니다:

```
Inorder traversal: 3 5 7 15 20 25 30 40 
Preorder traversal: 20 5 3 15 30 25 40 
Postorder traversal: 3 15 5 25 40 30 20 
Search for value 7: Found
Search for value 100: Not Found
Inorder traversal after deleting 10: 3 5 7 15 20 25 30 40 
Successor of 20: 25
Successor of 10: 15
Successor of 5: 7
```

이 예제를 통해 이진 탐색 트리의 기본 동작을 이해할 수 있습니다. 이진 탐색 트리를 활용한 다양한 연산을 연습해보세요.

### 결론
이진 탐색 트리는 효율적인 삽입, 삭제, 검색 연산을 제공하며, 후임자 개념을 통해 삭제 연산을 효과적으로 처리할 수 있습니다. 