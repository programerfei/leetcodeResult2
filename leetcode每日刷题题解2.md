# leetcode每日刷题题解

## 1、剑指offer 68-1（简单）

首先，该二叉树为二叉搜索树，我们观察二叉搜索树有以下规律：

1.若根节点的值大于两个节点得到值，那么说明这两个节点在根节点的左子树上。

2.若根节点的值小于两个节点的值，那么说明这两个节点位于根节点的右子树上。

若根节点的值等于两个节点中的一个节点的值，那么这个节点就是这两个节点的父节点，即自身为两个节点二点父节点。

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null){
            return null;
        }
        TreeNode node=root;
        while(true){
            if(node.val>p.val&&node.val>q.val){
                node=node.left;
            }else if(node.val<p.val&&node.val<q.val){
                node=node.right;
            }else{
                break;
            }
        }
        return node;
    }
}
```

## 2、剑指offer 57(简单)

首先，该数组为递增数组。

利用 HashMap 可以通过遍历数组找到数字组合，时间和空间复杂度均为 O(N)O(N) ；
注意本题的 numsnums 是 排序数组 ，因此可使用 双指针法 将空间复杂度降低至 O(1)O(1) 。

```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i = 0, j = nums.length - 1;
        while(i < j) {
            int s = nums[i] + nums[j];
            if(s < target) i++;
            else if(s > target) j--;
            else return new int[] { nums[i], nums[j] };
        }
        return new int[0];
    }
}

```

