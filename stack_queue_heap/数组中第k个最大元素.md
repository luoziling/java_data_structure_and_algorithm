# 数组中第k个最大元素

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

**暴力解法**：将数组排序，返回第n-k个元素

第k大就是从排序号的数组中从后向前数k个数

**堆解法**

第k大的元素，也就是说在k前面的数字都比k小，是不是很像堆的结构，一个小顶堆，堆顶就是第k个最大的元素

- 遍历数组，构建一个大小为k的小顶堆，向堆中插入元素
- 如果插入的元素个数小于k就向堆中插入元素
- 如果满了还要继续插入元素将待插入元素与堆顶元素比较如果大于堆顶元素就将堆顶元素出堆并加入新元素
- 若小于堆顶元素则不管继续遍历
- 最后返回堆顶元素即可

## 代码

```java
import java.util.Arrays;

/**
 * @author Satsuki
 * @time 2019/11/10 18:19
 * @description:
 * 暴力解法
 */
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        // 排序
        Arrays.sort(nums);
        // 返回
        return nums[nums.length-k];
    }
}



import java.util.PriorityQueue;

/**
 * @author Satsuki
 * @time 2019/11/10 18:19
 * @description:
 * 堆解法
 */
public class Solution2 {
    // 构建大顶堆
    private PriorityQueue<Integer> maxHeap;
    public int findKthLargest(int[] nums, int k) {
        maxHeap = new PriorityQueue<>();
        for (int i = 0; i < nums.length; i++) {
            if (maxHeap.size()<k){
                maxHeap.offer(nums[i]);
            }else if (nums[i]>maxHeap.peek()){
                maxHeap.poll();
                maxHeap.offer(nums[i]);
            }
        }
        return maxHeap.peek();
    }
}

```

