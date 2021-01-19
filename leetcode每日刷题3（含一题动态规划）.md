# leetcode每日刷题3（含一题动态规划）

## 1、剑指offer 11（简单）

这道题其实是二分法的一个变型，这个数组内容是一个递增序列的旋转，其实就是二分法的应用，不过在判断条件上有一些差别。

我们还是要注意中等点的划分，使用：**int mid=left+(right-left)/2;**而不是：**int mid=(right+left)/2;**是为了防止溢出。

```Java
class Solution {
    public int minArray(int[] numbers) {
        //二分查找
        int left=0;
        int right=numbers.length-1;
        while(left<right){
            int mid=left+(right-left)/2;
            int midValue=numbers[mid];
            if(midValue<numbers[right]){
                right=mid;;
            }else if(midValue>numbers[right]){
                left=mid+1;
            }else{
                right-=1;
            }
        }
        return numbers[left];
    }
}
```

## 2、剑指offer 37 （困难）

这道题是二叉树的序列化与反序列化。

1.首先对二叉树进行序列化时相对简单的，直接使用二叉树的层序遍历即可。

2.具体的序列化格式按照题目的要求进行即可，此处我们使用StringBuilder和LinkedList。

3.注意，若使用leetcode的实例，此时我们序列化真正的结果是：**1,2,3,null,null,4,5,null,null,null,null]**。



1.反序列化的时候，我们首先使用String的split()方法将整个序列化的字符串分割成字符串数组。

2.我们将字符串数组的第零个元素压入队列中，从第一个元素开始。

3.若数组元素不为"null"，则我们根据值创建树节点，并将其连接。

4.若数组元素为“null”,则我们直接将指针i ++。

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
public class Codec {

    public String serialize(TreeNode root) {
        if(root == null) return "[]";
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left);
                queue.add(node.right);
            }
            else res.append("null,");
        }
        res.deleteCharAt(res.length() - 1);
        res.append("]");
        return res.toString();
    }

    public TreeNode deserialize(String data) {
        if(data.equals("[]")) {
            return null;
        }
        String[] strs = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(strs[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        //字符串数组的第零个元素已经进入了队列，所以从第一个元素开始
        int i = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(!strs[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(strs[i]));
                queue.add(node.left);
            }
            i++;
            if(!strs[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(strs[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## 3、剑指offer 47（中等）

这是一道动态规划问题。

具体的解题思路也是和常规的动态规划问题很相似。

我们定义一个二维数组dp[][]，用于存放每次计算最大路径的值。

dp数组的初始化是个很重要的问题：地图的出发点礼物的最大值应该为本身，即dp [0] [0]=gird[0] [0]。

其次，我们考虑其边界值，边界值的礼物最大值dp [i] [j] 应该为当前的礼物价值加上上一个dp [i] [j-1]的值（i==0&&j!=0）；同理，当（i!=0&&j==0）时，情况也相同。

最后便是状态转移函数问题，我们要达到地图的右下角，我们有两个途径到达，dp [i-1] [j],dp[i] [j-1]。那么我们只要找到这两个中的较大值再加上此时礼物的价值便可得到最终的结果。

```Java
class Solution {
    public int maxValue(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        int[][] dp=new int[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0&&j==0){
                    dp[i][j]=grid[i][j];
                }else if(i==0&&j!=0){
                    dp[i][j]=dp[i][j-1]+grid[i][j];
                }else if(j==0&&i!=0){
                    dp[i][j]=dp[i-1][j]+grid[i][j];
                }else{
                    dp[i][j]=Math.max(dp[i-1][j],dp[i][j-1])+grid[i][j];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

