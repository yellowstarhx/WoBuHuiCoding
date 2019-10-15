# Struggling for LeetCode Java Solution  
  
>来个Offer吧！秋梨膏！

## Binary Search
[low, high)  
lowerBound  
upperBound  

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
### [445](https://leetcode.com/problems/add-two-numbers-ii/). Add Two Numbers II  
Oct.14, 2019 
>Runtime: 3 ms, faster than 68.59% of Java online submissions for Add Two Numbers II.  
>Memory Usage: 45.4 MB, less than 64.71% of Java online submissions for Add Two Numbers II.  
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
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        }
        // sol 1 : reverse
        // sol 2 : stack
        Deque<Integer> s1 = new LinkedList<>();
        Deque<Integer> s2 = new LinkedList<>();
        while (l1 != null) {
            s1.offerLast(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            s2.offerLast(l2.val);
            l2 = l2.next;
        }
        ListNode next = null;
        int carry = 0;
        ListNode cur = null;
        while (!s1.isEmpty() || !s2.isEmpty() || carry != 0) {
            // don't forget carry : [5], [5] -> [1, 0]
            int sum = carry;
            if (!s1.isEmpty()) {
                sum += s1.pollLast();
            }
            if (!s2.isEmpty()) {
                sum += s2.pollLast();
            }
            cur = new ListNode(sum);
            carry = cur.val / 10;
            cur.val %= 10;
            cur.next = next;
            next = cur;
        }
        return cur;
    }
}
```
### [24](https://leetcode.com/problems/swap-nodes-in-pairs/). Swap Nodes in Pairs  
Oct.14, 2019
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Swap Nodes in Pairs.  
>Memory Usage: 34.5 MB, less than 100.00% of Java online submissions for Swap Nodes in Pairs.  
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
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            ListNode next = cur.next;
            cur.next = next.next; 
            next.next = cur;
            prev.next = next;   // later element in last round. Important !
            prev = cur;
            cur = cur.next;
        }
        return dummy.next;
    }
}
```
### [206](https://leetcode.com/problems/reverse-linked-list/). Reverse Linked List [Easy]
Oct.14 ,2019  
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Reverse Linked List.  
>Memory Usage: 37.1 MB, less than 98.92% of Java online submissions for Reverse Linked List.  
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
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode prev = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```
## Greedy

### 218. Skyline Problem  
Oct 14, 2019 扫描线
```java
```
