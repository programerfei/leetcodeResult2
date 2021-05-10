# leetcode 34 [ 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

首先，这一题因为是在有序数组中查询目标元素的区间，那么我们自然而然的想到使用二分法进行查询目标元素，我们也就是使用二分查找的基本模板，最主要的问题在于找到目标值后查找相关的区间：

我的方法是设置两个指针进行向左和向右进行遍历：

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] arr=new int[2];
        arr[0]=-1;
        arr[1]=-1;
        if(nums.length==0){
            return arr;
        }
        int left=0;
        int right=nums.length-1;
        int i=-1;
        int j=-1;
        
        while(left<=right){
            int mid=(right-left)/2+left;
            if(target<nums[mid]){
                right=mid-1;
            }else if(target>nums[mid]){
                left=mid+1;
            }else{
                //通过二分查找找到等于目标值的数值，查找区间
                
                int temp1=mid;
                int temp2=mid;
                while(temp1-1>=0&&nums[temp1-1]==target){
                    temp1--;
                    //i=temp1;
                }
                while(temp2+1<nums.length&&nums[temp2+1]==target){
                    temp2++;
                    //j=temp2;
                }
                i=temp1;
                j=temp2;
                break;
            }
        }
        
        arr[0]=i;
        arr[1]=j;
        return arr;
    }
}
```

