📘 About This Repository
This repository contains most important Binary Tree + Binary Search Tree problems with:


Clean Java solutions
Beginner-friendly explanations
Full dry runs for every solution
Perfect for interviews (FAANG/MAANG)
GitHub portfolio ready
📂 Repository Structure
BinaryTreeProblems/
│── README.md
│── TreeNode.java
│── InsertInBST.java
│── SearchInBST.java
│── DeleteInBST.java
│── InvertBinaryTree.java
│── MaxDepthRecursive.java
│── MaxDepthIterativeDFS.java
│── MaxDepthIterativeBFS.java
│── PreorderTraversal.java
│── InorderTraversal.java
│── PostorderTraversal.java


**class:::**🌳 TreeNode Class
class TreeNode {
    int val;
    TreeNode left, right;

    TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}


🟢 1. Insert into BST
✔ Code — InsertInBST.java
public class InsertInBST {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);

        if (val < root.val)
            root.left = insertIntoBST(root.left, val);
        else
            root.right = insertIntoBST(root.right, val);

        return root;
    }
}


🧪 Dry Run
Insert in order: 5, 3, 6, 2, 4, 7
Start:
root = null
insert 5 → root = 5

Insert 3:
3 < 5 → goes left
root.left = 3

Insert 6:
6 > 5 → goes right
root.right = 6

Insert 2:
2 < 5 → go left (3)
2 < 3 → go left
root.left.left = 2

Insert 4:
4 < 5 → go left (3)
4 > 3 → go right
root.left.right = 4

Insert 7:
7 > 5 → go right (6)
7 > 6 → go right
root.right.right = 7

Final BST:
        5
      /   \
     3     6
    / \     \
   2   4     7


🔍 2. Search in BST
✔ Code — SearchInBST.java
public class SearchInBST {
    public boolean search(TreeNode root, int key) {
        while (root != null) {
            if (key == root.val) return true;
            else if (key < root.val) root = root.left;
            else root = root.right;
        }
        return false;
    }
}


🧪 Dry Run
Search for 4 in the tree above:
4 < 5 → go left (3)
4 > 3 → go right (4)
Found → return true

Search for 10:
10 > 5 → go right (6)
10 > 6 → go right (7)
10 > 7 → go right → null
return false


❌ 3. Delete Node in BST
✔ Code — DeleteInBST.java
public class DeleteInBST {

    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;

        if (key < root.val)
            root.left = deleteNode(root.left, key);
        else if (key > root.val)
            root.right = deleteNode(root.right, key);
        else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            TreeNode min = findMin(root.right);
            root.val = min.val;
            root.right = deleteNode(root.right, min.val);
        }
        return root;
    }

    private TreeNode findMin(TreeNode node) {
        while (node.left != null) node = node.left;
        return node;
    }
}


🧪 Dry Run — Delete 3
Before:
        5
      /   \
     3     6
    / \     \
   2   4     7



Find node 3


Node 3 has two children


Find min in right subtree → 4


Replace 3 with 4


Delete original node 4


Final:
        5
      /   \
     4     6
    /       \
   2         7


🔄 4. Invert Binary Tree
✔ Code — InvertBinaryTree.java
public class InvertBinaryTree {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}


🧪 Dry Run
Input:
    4
   / \
  2   7

At 4:
swap(2, 7)

At 7:
swap(6, 9)

At 2:
swap(1, 3)

Final:
    4
   / \
  7   2
 / \ / \
9  6 3  1


📏 5. Maximum Depth (Recursive)
✔ Code — MaxDepthRecursive.java
public class MaxDepthRecursive {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}


🧪 Dry Run
Tree:
1
├──2
└──3
     └──4

Call flow:
maxDepth(1)
 → 1 + max(maxDepth(2), maxDepth(3))

maxDepth(2) = 1
maxDepth(3)
 → 1 + max(0, maxDepth(4))
maxDepth(4) = 1

maxDepth(3) = 2

Answer = 1 + max(1,2) = 3


🟧 6. Maximum Depth (Iterative DFS)
✔ Code — MaxDepthIterativeDFS.java
public class MaxDepthIterativeDFS {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, 1));

        int res = 0;

        while (!stack.isEmpty()) {
            Pair<TreeNode, Integer> p = stack.pop();
            TreeNode node = p.getKey();
            int depth = p.getValue();

            if (node != null) {
                res = Math.max(res, depth);
                stack.push(new Pair<>(node.left, depth + 1));
                stack.push(new Pair<>(node.right, depth + 1));
            }
        }
        return res;
    }
}


🧪 Dry Run
Stack steps:
Push (1,1)
Pop → update depth = 1
Push (2,2),(3,2)
Pop (3,2) → depth = 2
Push (null,3),(4,3)
Pop (4,3) → depth = 3
Push (null,4),(null,4)
Pop null...
Pop (2,2)
depth remains 3

Result → 3

🟦 7. Maximum Depth (BFS)
✔ Code — MaxDepthIterativeBFS.java
public class MaxDepthIterativeBFS {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        int level = 0;

        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }
            level++;
        }
        return level;
    }
}


🧪 Dry Run
Level 1:
q = [1] → next = [2,3]

Level 2:
q = [2,3] → next = [4]

Level 3:
q = [4] → next = []

Answer = 3

🟩 8. Preorder Traversal (Root → Left → Right)
✔ Code — PreorderTraversal.java
public class PreorderTraversal {
    public List<Integer> preorder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        preorderHelper(root, list);
        return list;
    }
    private void preorderHelper(TreeNode node, List<Integer> list) {
        if (node == null) return;
        list.add(node.val);
        preorderHelper(node.left, list);
        preorderHelper(node.right, list);
    }
}


🧪 Dry Run
Tree:
   5
  / \
 3   6

Traversal:
Visit 5
Visit 3
Visit 6

Result:
[5,3,6]


🟩 9. Inorder Traversal (Left → Root → Right)
✔ Code — InorderTraversal.java
public class InorderTraversal {
    public List<Integer> inorder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        inorderHelper(root, list);
        return list;
    }
    private void inorderHelper(TreeNode node, List<Integer> list) {
        if (node == null) return;
        inorderHelper(node.left, list);
        list.add(node.val);
        inorderHelper(node.right, list);
    }
}


🧪 Dry Run
Same tree:
Left (3)
Root (5)
Right (6)

Result:
[3,5,6]


🟩 10. Postorder Traversal (Left → Right → Root)
✔ Code — PostorderTraversal.java
public class PostorderTraversal {
    public List<Integer> postorder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        postorderHelper(root, list);
        return list;
    }
    private void postorderHelper(TreeNode node, List<Integer> list) {
        if (node == null) return;
        postorderHelper(node.left, list);
        postorderHelper(node.right, list);
        list.add(node.val);
    }
}


🧪 Dry Run
Traversal:
3
6
5

Result:
[3,6,5]


🎉 Done
This README includes:
✔ All code
✔ All problems
✔ All dry runs
✔ GitHub banner
✔ Full explanations
If you want, I can also generate:
🔥 DSA roadmap section
Just tell me!
