##### 描述
###### 编写一个程序，找到两个单链表相交的起始节点。
###### 如下面两个链表
![图示](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png "两个链表相交于c1节点")
###### 示例1
![示例1](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png "相交于8")
> 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
###### 示例2
![示例2](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png "相交于2")
> 输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
###### 示例3
![示例3](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png "两个链表不相交")
> 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
##### 注意
- 如果两个链表没有交点，返回`null`
- 在返回结果后，两个链表仍须保持原有的结构
- 可假定整个链表结构中没有循环
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存

##### 解决方案
###### 暴力破解
###### 最简单的就是遍历两个链表。每个节点都做对比，相等表示两个相交
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    tmp := headB
    for headA != nil {
        headB = tmp
        for headB != nil {
            if headA == headB {
                return headA
            }
            headB = headB.Next
        }
        headA = headA.Next
    }
    return nil
}
// 时间复杂度 O(mn), 耗时564ms
// 空间复杂度 O(1), 内存占用6.6MB	
```
###### 哈希
###### 这个也很清楚，就是先遍历一个链表，保存所有节点。然后遍历第二个链表，如果节点在hash中存在则表示相交
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    var res *ListNode
    dict := make(map[*ListNode]bool)
    for headA != nil {
        dict[headA] = true
        headA = headA.Next
    }
    for headB != nil {
        _, exist := dict[headB]
        if exist {
            res = headB
            break
        }
        headB = headB.Next
    }
    return res
}
// 时间复杂度 O(m+n) 耗时52ms
// 空间复杂度 O(m)或者O(n) 内存占用8.5MB
```
###### 双指针
###### 首先判断是否相交，假如相交，那么headA和headB的末尾节点是相等。不相等则表示不相交。
###### 我们可以依次遍历headA和headB两个链表，并把链表的长度记录下来，计算长度差`diffLen`，既然链表有相交，那么我们可以让较长的链表走`diffLen`的长度，然后在和较短的链表同步遍历，当两个值相等时，自然就是相交点。
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    travelA := headA
    travelB := headB
    var lenA,lenB int
    for headA != nil {
        headA = headA.Next
        lenA++
    }
    for headB != nil {
        headB = headB.Next
        lenB++
    }
    diffLen := Abs(lenA - lenB)
    for i := 0; i < diffLen; i++ {
        if lenA > lenB {
            travelA = travelA.Next
        } else {
            travelB = travelB.Next
        }
    }
    for tarvelA != travelB {
        travelA = travelA.Next
        travelB = travelB.Next
    }
    return travelA
}
// 时间复杂度 O(m+n) 耗时 64ms
// 空间复杂度 O(1) 内存占用 7.5MB
```