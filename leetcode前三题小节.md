# leetcode前三题小节

# 两数之和

<https://leetcode-cn.com/problems/two-sum/>

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 核心思想

把字符数组转为map或者set或linkedlist这些带有contains的Collection集合

把找两个数相加得到目标target转为

先找到了一个数然后另一个数就是target-先找到的数

然后在保存字符数组的Collection中调用.contains方法如果在数组中包含另一个数字即刻返回否则重复寻找

## 代码

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
//            map.put(i,nums[i]);
            // 最好使用key保存题目中的数组内容，防止重复
            map.put(nums[i],i);
        }
        // 遍历数组并且在哈希表中找出是否存在另一个符合要求的数字得到两数之和为target
        int another;
        for (int i = 0; i < nums.length; i++) {
            another = target - nums[i];
            if (map.containsKey(another)&&map.get(another)!=i){
                return new int[]{i,map.get(another)};
            }
        }
        return null;
    }
}
```

# 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 核心思想

在对链表进行操作的时候记得保存一个副本，对副本进行操作不要对链表直接操作，否则如题目的单向链表就无法进行像回找的操作

定义是否溢出

当两个链表有一个未达到最后就继续相加，如果其中一个达到最后为空则取0值即可

在全体加完后记得看看是否还溢出了

如果溢出要多加一个节点内容为1

注意特殊情况

## 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //定义返回的链表
        // 初始节点
        ListNode res = new ListNode(1);
        ListNode res1 = res;

        // 定义一个保存相加溢出的标识位
        boolean flag = false;
        int temp;// 相加的中间数

        //记录中间数据
        int v1, v2;
        // 当l1或者l2不为空就行
        // 因为可能两个链表的代表的数据位数是不相等的
//        while (l1!=null&& l2!=null){
        while (l1 != null || l2 != null) {
            // 这里做一个判0以访数据位数不相等 比如321 + 3这种
            v1 = l1 == null ? 0 : l1.val;
            v2 = l2 == null ? 0 : l2.val;
            // 因为是倒序存储所以正好按照加法的顺序
            temp = v1 + v2;
            // 如果之前一次相加产生进位则要再加一
            if (flag) {
                temp += 1;
                // 重置flag
                flag = false;
            }
            // 如果相加大于等于10就有进位
            if (temp >= 10) {
                // 取模
                temp = temp % 10;
                // 进位标识置true
                flag = true;
            }
            res.next = new ListNode(temp);
            res = res.next;
            // l1 l2后移
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }

        }
        // 防止处理完后还需要进位 例如 998 + 2
        if (flag) {
            res.next = new ListNode(1);
        }

        //        return res.next;
        if (res1.next != null) {
            res1 = res1.next;
        }
        return res1;
    }
}
```



# 无重复字符的最长字串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 核心思想

滑动窗口

定义一个LinkList作为滑动窗口即可

## 代码

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 字符串为空直接返回 ：首先就需要是对该进行操作的值进行一个判定以防意外
        if (s.length() == 0){
            return 0;
        }
        LinkedList<Character> charList = new LinkedList<>();
        int max = 0;
        for (int i = 0; i < s.length(); i++) {
            while (charList.contains(s.charAt(i))){
                // 一定要从头节点开始移除， 滑动窗口
                charList.removeFirst();
            }
            charList.add(s.charAt(i));
            if (charList.size()>max){
                max = charList.size();
            }
        }
        return max;
    }
}
```

# 用队列实现栈

## 核心思想

定义一个LinkList作为队列实现栈

push直接添加即可

pop就移除最后一个并返回

top就getLast并返回（不移除

## 代码

```java
class MyStack {
    LinkedList<Integer> list;

    /** Initialize your data structure here. */
    public MyStack() {
        list = new LinkedList<>();

    }

    /** Push element x onto stack. */
    public void push(int x) {
        // 按顺序进栈
        list.add(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        if (!empty()){
            // 逆序出栈
//            return list.removeFirst();
            return list.removeLast();
        }
        return -1;
    }

    /** Get the top element. */
    public int top() {
        if (!empty()){
            return list.getLast();
        }
        return -1;
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return list.size()==0;
    }
}
/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

方法三 （一个队列， 压入 - O(n)O(n)， 弹出 - O(1)O(1)）
上面介绍的两个方法都有一个缺点，它们都用到了两个队列。下面介绍的方法只需要使用一个队列。

算法

压入（push）

当我们将一个元素从队列入队的时候，根据队列的性质这个元素会存在队列的后端。

但当我们实现一个栈的时候，最后入队的元素应该在前端，而不是在后端。为了实现这个目的，每当入队一个新元素的时候，我们可以把队列的顺序反转过来。

![](https://pic.leetcode-cn.com/558b9e9258a8ba35c6456ea714d05f55d35da3c3306bef8fa47099093a3ab5b7-file_1561370741978)

Figure 5. Push an element in stack

Java

```java
private LinkedList<Integer> q1 = new LinkedList<>();

// Push element x onto stack.
public void push(int x) {
    q1.add(x);
    int sz = q1.size();
    while (sz > 1) {
        q1.add(q1.remove());
        sz--;
    }
}
```

复杂度分析

时间复杂度：O(n)O(n)
这个算法需要从 q1 中出队 nn 个元素，同时还需要入队 nn 个元素到 q1，其中 nn 是栈的大小。这个过程总共产生了 2n + 12n+1 步操作。链表中 插入 操作和 移除 操作的时间复杂度为 O(1)O(1)，因此时间复杂度为 O(n)O(n)。

空间复杂度：O(1)O(1)

弹出（pop）

最后一个压入的元素永远在 q1 的前端，这样的话我们就能在常数时间内让它 出队。

Java

```java
// Removes the element on top of the stack.
public void pop() {
    q1.remove();
}
```

复杂度分析

时间复杂度：O(1)O(1)
空间复杂度：O(1)O(1)
判断空（empty）

q1 中包含了栈中所有的元素，所以只需要检查 q1 是否为空就可以了。

Java


```java
// Return whether the stack is empty.
public boolean empty() {
    return q1.isEmpty();
}
```

时间复杂度：O(1)O(1)
空间复杂度：O(1)O(1)

取栈顶（top）

栈顶元素永远在 q1 的前端，直接返回就可以了。

Java

```java
// Get the top element.
public int top() {
    return q1.peek();
}
```

时间复杂度：O(1)O(1)
空间复杂度：O(1)O(1)

作者：LeetCode
链接：https://leetcode-cn.com/problems/implement-stack-using-queues/solution/yong-dui-lie-shi-xian-zhan-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。