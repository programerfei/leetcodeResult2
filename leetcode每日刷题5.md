# leetcode每日刷题

## 1、leetcode 617（简单）合并二叉树

这一题从根节点合并两颗二叉树，我们使用深度优先搜索进行解决。

```Java
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
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1==null){
            return t2;
        }
        if(t2==null){
            return t1;
        }
        TreeNode node=new TreeNode(t1.val+t2.val);
        node.left=mergeTrees(t1.left,t2.left);
        node.right=mergeTrees(t1.right,t2.right);
        return node;
    }
}
```

## 2、leetcode538 （中等）

本题中要求我们将每个节点的值修改为原来的节点值加上所有大于它的节点值之和。这样我们只需要反序中序遍历（右-根-左）该二叉搜索树，记录过程中的节点值之和，并不断更新当前遍历到的节点的节点值，即可得到题目要求的累加树。

```Java
class Solution {
    int sum = 0;

    public TreeNode convertBST(TreeNode root) {
        if (root != null) {
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
}
```

## 3、leetcode 169 (简单) 多数元素

这一题我最先想到的是哈希表，当然哈希表完全可以做。

但是我们可以根据一个多数元素占数组一半以上的特性，将其排序然后得到中间元素的值，即为所求。

```Java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
```

