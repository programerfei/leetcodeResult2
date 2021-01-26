# leetcode每日刷题7

## 1、leetcode

该问题可以转化为：两个树在什么情况下互为镜像？

如果同时满足下面的条件，两个树互为镜像：

它们的两个根结点具有相同的值
每个树的右子树都与另一个树的左子树镜像对称

通过「同步移动」两个指针的方法来遍历这棵树，pp 指针和 qq 指针一开始都指向这棵树的根，随后 p 右移时，q 左移，p 左移时，q 右移。每次检查当前 p 和 q 节点的值是否相等，如果相等再判断左右子树是否对称。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }

    public boolean check(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
    }
}
```

**注：**我本身的想法时通过中序遍历得到遍历结果，然后通过遍历结果进行检查是否是一个回文串，从而判断二叉树是否为对称二叉树，但是在提交的时候卡在了192/195的案例处，因为**[1,2,2,2,null,2]**的二叉树不是对称的，但是通过中序遍历得到的结果集确实一个回文串，所以该方法是有问题的。

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

 //中序遍历
class Solution {
    public boolean isSymmetric(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        order(root,list);
        int i=0;
        int j=list.size()-1;
        while(i<j){
            if(list.get(i)!=list.get(j)){
                return false;
            }else{
                i++;
                j--;
            }
        }
        return true;
    }
    public void order(TreeNode root,List<Integer> list){
        if(root==null){
            return;
        }
        if(root.left!=null){
            order(root.left,list);
        }
        list.add(root.val);
        if(root.right!=null){
            order(root.right,list);
        }
    }
}
```

## 2、leetcode 221（中等）最大正方形

这是一题动态规划问题，我们要寻找包含‘1’的最大正方形。最大的问题是找状态转移方程。

我们定义一个dp[] []二维数组，dp[i] [j]为该点包含‘1’最大正方形的边长。

边界值我们好确定，若是包含‘1’，则该处的边长也就是dp[i] [j]=1。

然后找状态转移方程，我们寻找的边长就是左边，上边，左上角最小值加一，即：dp[i] [j]=Math.min(Math.min(dp[i-1] [j],dp[i] [j-1]),dp[i-1] [j-1])+1;

最后返回最大边的乘积即可。

```Java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if(matrix==null){
            return 0;
        }
        int maxSize=0;
        int m=matrix.length;
        int n=matrix[0].length;
        int[][] dp=new int[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='1'){
                    if(i==0||j==0){
                        dp[i][j]=1;
                    }else{
                        dp[i][j]=Math.min(Math.min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
                    }
                    //System.out.println(maxSize);
                    maxSize = Math.max(maxSize, dp[i][j]);
                }
            }
        }
        //System.out.println(maxSize);
        return maxSize*maxSize;
    }
}
```

## 3、leetcode 238（中等）除自身以外数组的乘积

1、首先，因为题目限制不能使用除法，且要求时间复杂度为O(N)，所以我们不能使用将数组乘积然后除去自身来解决这个问题。

2、最常用的方法是左右数组列表的方法。

3、具体就是我们定义两个数组，数组的初始值都为1。

4、其中一个数组left从左到右计算left的前缀数据乘以原数组nums的前一个数据。

5、另一个数组right从右到左计算right的前缀数据和nums的前一个数据。

6、相同位置两个数组的乘积即为我们的答案。

```Java

class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] left=new int[nums.length];
        int[] right=new int[nums.length];

        int[] result=new int[nums.length];

        left[0]=1;
        for(int i=1;i<nums.length;i++){
            left[i]=left[i-1]*nums[i-1];
        }

        right[nums.length-1]=1;
        for(int i=nums.length-2;i>=0;i--){
            right[i]=right[i+1]*nums[i+1];
        }

        for(int i=0;i<nums.length;i++){
            result[i]=left[i]*right[i];
        }
        return result;
    }
}
```

