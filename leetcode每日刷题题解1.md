# leetcode每日刷题题解

## 1、剑指offer 32-1（中等）

这道从上到下打印二叉树，我们看案例，我们选择使用广度优先搜索做这道题。

这是很简单的广度优先搜索，也是很经典的广度优先搜索。

广度优先搜索我们常用的便是使用一个ArrayList集合和一个Queue队列，也就是我们所说的二叉树的层序遍历。

详情看具体代码：

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
 //广度优先搜索
class Solution {
    public int[] levelOrder(TreeNode root) {
        if(root==null){
            return new int[0];
        }
        
        ArrayList<Integer> list=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();

        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode node=queue.poll();
            list.add(node.val);
            if(node.left!=null){
                queue.offer(node.left);
            }
            if(node.right!=null){
                queue.offer(node.right);
            }
        }
        int[] arr=new int[list.size()];
        for(int i=0;i<list.size();i++){
            arr[i]=list.get(i);
        }
        return arr;
    }
}
```

## 2、剑指offer 32-2（简单）

这道题和上面的题本质上是一样的，但是多了一个输出要求，我们需要将同一层的节点按从左到右的顺序打印，每一层打印到一行。

这样的话我们使用List集合便能够解决，即List<list<Integer>>。因为和上面的代码没有什么本质区别，在此不做具体展示。

## 3、剑指offer 32-3（中等）

这道题是上面两道题的结合。

我们分两步进行分析。

首先，我们需要层序遍历二叉树，这个实现再第一道题中已经进行了实现。

其次，我们需要在层数为奇数是正向存储在某一数据结构中，而在层数为偶数时反向将数据存储在某一数据结构中，这便是第二道题的实现。

第一步没有问题，正常的二叉树层序遍历。

第二步我们要找一个数据结构进行存储的数据，我们使用双端队列，由LinkdedList实现，在尾端加入数据由addFirst()，实现，在头部加入数据由addLast()实现。

最后，我们一定要注意如何控制和计算遍历的层数，在此处我们使用了嵌套循环，在以下的具体实现代码中我们最好使用一个变量暂时存储每一次Queue队列的大小；还有就是，我们使用最终的List的大小进行模运算来计算我们遍历二叉树的层数。

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null){
            return new ArrayList();
        }
        
        List<List<Integer>> res=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            LinkedList<Integer> list=new LinkedList<>();
            int size=queue.size();
            for(int i=0;i<size;i++){
                TreeNode node=queue.poll();
                if(res.size()%2==0){
                    list.addLast(node.val);
                }else{
                    list.addFirst(node.val);
                }
                if(node.left!=null){
                    queue.offer(node.left);
                }
                if(node.right!=null){
                    queue.offer(node.right);
                }
            }
            res.add(list);
        }
        return res;
    }
}
```

## 4、剑指offer 33（中等）

做提前，我们先明确两个知识点：

1.后序遍历：左、右、中。

2.二叉搜索树：左子树所有节点的值小于根节点的值，右子树所有节点的值大于根节点的值，左子树所有节点的值小于右子树所有节点的值。

这道题刚开始毫无思路，去了解了以下人家的思路。

我们将后序遍历的结果做相反处理，这样我们就能都得到“根，右，左”的遍历结果，我们可以看到和前序遍历是很像的。

我们观察后续遍历的**倒序**结果：

**如果arr[i]<arr[i+1]，那么arr[i+1]一定是arr[i]的右子节点**

**如果arr[i]>arr[i+1]，那么arr[i+1]一定是arr[0]……arr[i]中某个节点的左子节点，并且这个值是大于arr[i+1]中最小的**

如果栈为空，就把当前元素压栈。如果栈不为空，并且当前元素大于栈顶元素，说明是升序的，那么就说明当前元素是栈顶元素的右子节点，就把当前元素压栈，如果一直升序，就一直压栈。当前元素小于栈顶元素，说明是倒序的，说明当前元素是某个节点的左子节点，我们目的是要找到这个左子节点的父节点，就让栈顶元素出栈，直到栈为空或者栈顶元素小于当前值为止，其中最后一个出栈的就是当前元素的父节点。

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        Stack<Integer> stack = new Stack<>();
        int root = Integer.MAX_VALUE;
        for(int i = postorder.length - 1; i >= 0; i--) {
            if(postorder[i] > root){
                return false;
            } 
            while(!stack.isEmpty() && stack.peek() > postorder[i]){
                root = stack.pop();
            }
            stack.add(postorder[i]);
        }
        return true;
    }
}

```

