# 环形链表(Linked List Cycle)

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)


示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)


进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

利用set与contains方法解决

遍历链表并且加入set在加入前判断时候已有这个节点如果有说明该链表是环形链表

利用快慢指针

设置两个指针一个慢指针一个快指针慢指针在遍历时每次走一步，快指针在遍历时每次走两步，若有环则两个指针一定会相遇。若相遇返回true即可

## 代码

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        // 暴力解法
        HashSet<ListNode> set = new HashSet<>();
        while (head!=null){
            if (set.contains(head)){
                return true;
            }
            set.add(head);
            head = head.next;
        }
        return false;
    }
}
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 class ListNode {
      int val;
      ListNode next;
      ListNode(int x) {
          val = x;
          next = null;
      }
  }

/**
 * @author Satsuki
 * @time 2019/11/13 18:42
 * @description:
 * 利用跑道快慢指针
 * 如果有环那么跑的快的一定会再次追上跑得慢的
 * 追上了就说明有环
 */
public class Solution2 {
    public boolean hasCycle(ListNode head) {
        if (head == null||head.next == null){return false;}
        ListNode slow = head;
        ListNode fast = head;
        while (fast!=null&&fast.next!=null){
            // 慢的走一步
            slow = slow.next;
            // 快的走两步
            fast = fast.next.next;

            if (slow == fast){
                return true;
            }

        }

        return false;
    }
}
```