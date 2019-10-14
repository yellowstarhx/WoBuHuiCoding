# Learning Coding: Solutions for LeetCode using Java  
  
>我变秃了，也变强了。

## Binary Search
[low, high)

## List

### 2. Add two Numbers  
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
        int digit = 0;
        int carry = 0;
        ListNode dummyHead = new ListNode(0);
        ListNode cur = dummyHead;
        while (l1 != null || l2 != null) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            digit = x + y + carry;
            carry = digit / 10;
            cur.next = new ListNode(digit % 10);
            cur = cur.next;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry > 0) {
            cur.next = new ListNode(carry);
        }
        return dummyHead.next;
    }
}
```  
### 445. Add Two Numbers II  
```java
```

## Greedy

### 218. Skyline Problem  
Oct 14, 2019
```java
```
