# Diameter of Binary Tree

Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

## Solutions

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```

### DFS (Recursive)

An important part of this problem is understanding the **height** of a tree (using DFS) and how to get the **diameter**.

You can perform a _post-order_ DFS traversal to work through each node in the tree, receiving the height at each step, and calculating the max diameter using a global variable.

```java
class Solution {
    private int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        dfsHeight(root);
        return maxDiameter;
    }

    private int dfsHeight(TreeNode node) {
        if (node == null) return 0;

        int leftHeight = dfsHeight(node.left);
        int rightHeight = dfsHeight(node.right);

        int diameter = leftHeight + rightHeight;
        maxDiameter = Math.max(maxDiameter, diameter);

        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```
