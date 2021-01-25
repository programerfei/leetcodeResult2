# leetcode每日刷题6

## 1、leetcode 11（中等）盛最多水的容器

这一题是一题数组题，因为要考虑最多的盛水量，我们最先想到的应该是暴力法解决，但是因为暴力法的效率极低，我们应该想出一种比较优化的算法解决这个问题。

因为是数组问题，所以我们很快能想到我们使用双指针，大概捋一遍思路：我们使用双指针，每次计算双指针指向的容器成水量，然后使用一个变量max存储，然后指针指向的值小的一个数组的指针移动，再计算盛水量然后进行比较，选择较大的值做为max。

看一下的具体实现代码，思路很简单。

```Java
class Solution {
    public int maxArea(int[] height) {
        int left=0;
        int right=height.length-1;
        int max=0;
        while(left<right){
            int arrLength=height[left]<=height[right]?height[left]:height[right];
            int arrHeight=right-left;
            int temp=arrHeight*arrLength;
            if(max<temp){
                max=temp;
            }
            //System.out.println(max);
            if(height[left]<=height[right]){
                left++;
            }else{
                right--;
            }
        }
        return max;
    }
}
```

## 2、leetcode 104（简单）二叉树的最大深度

这一题是简单题，我们直接使用递归便可以解决。

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
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        } else {
            int leftHeight = maxDepth(root.left);
            int rightHeight = maxDepth(root.right);
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}
```

## 3、leetcode 198（中等）打家劫舍

这一题我们很自然的使用动态规划解题。

动态规划最重要的是找到相应的状态和状态转移方程，在做这一题的时候，我第一次是将状态转移方程找错以至于题出错。

首先，假设数组长度为n，我们找这一题的状态，我们要找到不触碰报警装置的情况下找到最大的金额，那么只有两种可能，要么是当偷窃到最后一家时拿到的总金额，要么是偷窃到倒数第二家时拿到的总金额。

其次，我们找状态转移方程：**dp[i]=Math.max(dp[i-2]+nums[i-1],dp[i-1]);(0=<i<=n)**，特别注意状态转移方程不是简单的**dp[i]=dp[i-2]+nums[i-1]**。

之后，我们确定初始值，即数组程度为零（没有家庭可偷窃），dp值为0；数组长度为1（只有一个家庭）dp值为这一个家庭的金额，即**dp[1]=nums[0]**；数组长度为二（有两个家庭）,dp值为两家较大的一家，即**dp[2]=Math.max(nums[0],nums[1])**

最后，我们返回**Math.max(dp[nums.length],dp[nums.length-1]);**

```Java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0){
            return 0;
        }
        if(nums.length==1){
            return nums[0];
        }
        if(nums.length==2){
            return Math.max(nums[0],nums[1]);
        }
        int[] dp=new int[nums.length+1];
        dp[0]=0;
        dp[1]=nums[0];
        dp[2]=Math.max(nums[0],nums[1]);
        for(int i=3;i<=nums.length;i++){
            dp[i]=Math.max(dp[i-2]+nums[i-1],dp[i-1]);
        }
        return Math.max(dp[nums.length],dp[nums.length-1]);
    }
}
```

## 3、leetcode 213（中等）打家劫舍2

这一题在上一题的基础上增加了一点难度，题目将其设置为了一个圈，具体题意大家可在leetcode上进行查看。

因为一个圈的缘故，我们选择第一家的时候就不能选择最后一家，选择最后一家就不能选择第一家，所以我们将这个数组分为两个数组：**含有第零个元素但不含有最后一个元素的数组arr1[]，含有最后一个元素但不含有第零个元素的数组arr2[]**，这样我们就会避免头尾相连从而出发警报装置，然后各自使用前一题的解法得到各自的结果，然后从中选出最大的一个即为最终结果。

```Java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0){
            return 0;
        }
        if(nums.length==1){
            return nums[0];
        }
        if(nums.length==2){
            return Math.max(nums[0],nums[1]);
        }
        // if(nums.length==3){
        //     return nums[1];
        // }

//将数组拆分成两个队列，一个包含第一个数，一个包含最后一个数
        int[] arr1=new int[nums.length-1];
        int[] arr2=new int[nums.length-1];
        for(int i=0;i<nums.length-1;i++){
            arr1[i]=nums[i];
        }
        for(int i=1;i<nums.length;i++){
            arr2[i-1]=nums[i];
        }
        // for(int i=0;i<arr2.length;i++){
        //     System.out.println(arr2[i]);
        // }
        // System.out.println(arr2.length);
        int[] dp1=new int[nums.length];
        int[] dp2=new int[nums.length];

        dp1[0]=0;
        dp1[1]=arr1[0];
        dp1[2]=Math.max(arr1[0],arr1[1]);
        for(int i=3;i<=nums.length-1;i++){
            dp1[i]=Math.max(dp1[i-2]+arr1[i-1],dp1[i-1]);
        }
        int max1=Math.max(dp1[nums.length-1],dp1[nums.length-2]);
        dp2[0]=0;
        dp2[1]=arr2[0];
        dp2[2]=Math.max(arr2[0],arr2[1]);
        for(int i=3;i<=nums.length-1;i++){
            dp2[i]=Math.max(dp2[i-2]+arr2[i-1],dp2[i-1]);
        }
        int max2=Math.max(dp2[nums.length-1],dp2[nums.length-2]);
        return Math.max(max1,max2);
    }
}
```

