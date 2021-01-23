# leetcode刷题

## 1、leetcode 563 （简单）二叉树的坡度

这一题我们的任务是计算整个树的坡度，我们使用深度优先搜索解决此问题。

我们创建一个方法实现节点坡度的计算。

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
    int result=0;
    public int findTilt(TreeNode root) {
        sum(root);
        return result;
    }

    public int sum(TreeNode node){
        if(node==null){
            return 0;
        }
        int left=sum(node.left);
        int right=sum(node.right);
        result+=Math.abs(left-right);
        return left+right+node.val;
    }
}
```

## 2、leetcode 392（简单）判断子序列

这一题本来想用动态规划解决，但是突然发现使用双指针可能在思路上更加的简单，所以使用了双指针进行解决此问题。

具体思路便是使用两个指针，分别指向两个字符串的头字符，我们逐个遍历，若两个指针指向的字符相等，那么我们就将两个字符串的指针都加一；若不想等，那么我们将长的字符串的指针加一，而短字符串的指针不发生移动。

遍历完成之后，我们判断短字符串的指针的长度是否等于短字符串的长度。

```Java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int m=s.length();
        int n=t.length();
        int i=0;
        int j=0;
        while(i<m&&j<n){
            if(s.charAt(i)==t.charAt(j)){
                i++;
            }
            j++;
        }
        if(i==m){
            return true;
        }else{
            return false;
        }
    }
}

```

## 3、leetcode 48 (中等) 旋转图像

这一题我的思路是先左右进行反转，然后左上-右下反转。这一个具体实现很好实现，在此不再贴代码。

## 4、leetcode 645（简单）

因为数组的值不一定是递增的，所以循环和递增的自然数比较是错误的，比如：

```Java
class Solution {
    public int[] findErrorNums(int[] nums) {
        if(nums.length==0){
            return new int[0];
        }
        int i=1;//自然数的第一个数
        int[] arr=new int[2];
        
        for(int j=0;j<nums.length;j++){
            if(nums[j]!=i){
                arr[0]=nums[j];
                arr[1]=i;
            }
            i++;
        }
        return arr;
    }
}
```

我们使用哈希表

```Java
class Solution {
    public int[] findErrorNums(int[] nums) {
        Map < Integer, Integer > map = new HashMap();
        int dup = -1, missing = 1;
        for (int n: nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        for (int i = 1; i <= nums.length; i++) {
            if (map.containsKey(i)) {
                if (map.get(i) == 2)
                    dup = i;
            } else
                missing = i;
        }
        return new int[]{dup, missing};
    }
}
```

