##### 描述
###### 给定一个链表，判断链表中是否有环。
###### 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
##### 示例
###### 示例1
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```
![示例1](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)
###### 示例2
```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```
![示例2](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)
###### 示例3
```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```
![示例3](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)
##### 进阶
###### 你能用 O(1)（即，常量）内存解决此问题吗？
##### 解决方案
###### 我们可以使用双指针来解答，不知大家是否记得小时候的数学题，`甲和已向相同的方法跑，甲的速度是已的2倍，围着操场跑，问甲和已什么时候相遇。`这类的问题,其实和我们这个题目有点像，我用两个指针（\*p,\*q），开始遍历,q的速度是p的两倍，如果这是一个循环链表，那么他们肯定会相遇。
###### 需要注意的是如果是一个空链表或者只有一个节点那么肯定不是循环链表
```go
func hasCycle(head *ListNode) bool {
    if head == nil || head.Next == nil {
        return false
    }
    p := head
    q := head.Next
    for p != q {
        if q == nil || q.Next == nil {
            return false
        }
        p = p.Next
        q = q.Next.Next
    }
    return true
}
```
