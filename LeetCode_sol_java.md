# Struggling for LeetCode (Java Solution)  
  
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
### [141](https://leetcode.com/problems/linked-list-cycle/). Linked List Cycle (Easy)
Oct.16,2019
```java
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
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```
### [142](https://leetcode.com/problems/linked-list-cycle-ii/). Linked List Cycle II (Medium)
Oct.16, 2019  
> Runtime: 0 ms, faster than 100.00% of Java online submissions for Linked List Cycle II.  
> Memory Usage: 34.1 MB, less than 95.79% of Java online submissions for Linked List Cycle II.  
```java
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
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode hare = hasCycle(head);
        if (hare == null) {
            return null;
        }
        // let r be the length of the circle
        // fast ptr walks n*r in the circle when it meets slow ptr
        // let f be the length before go into the circle
        // let c be the length slow ptr walks in the cirle
        // 2 * (f + c) = f + n * r + c
        // so, f = n * r - c
        ListNode tortoise = head;
        while (tortoise != hare) {
            tortoise = tortoise.next;
            hare = hare.next;
        }
        return tortoise;
    }
    
    private ListNode hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                return fast;
            }
        }
        return null;
    }
}
```
### [21](https://leetcode.com/problems/merge-two-sorted-lists/) Merge Two Sorted Lists (easy)
Oct 18, 2019
> Runtime: 0 ms, faster than 100.00% of Java online submissions for Merge Two Sorted Lists.  
>Memory Usage: 40.6 MB, less than 13.13% of Java online submissions for Merge Two Sorted Lists.  
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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        while (l1 != null && l2 != null) {
            ListNode min = null;
            if (l1.val <= l2.val) {
                min = l1;
                l1 = l1.next;
            } else {
                min = l2;
                l2 = l2.next;
            }
            prev.next = min;
            prev = prev.next;
        }
        prev.next = l1 == null ? l2 : l1;
        return dummy.next;
    }
}
```
### [23](https://leetcode.com/problems/merge-k-sorted-lists/) Merge k Sorted Lists (hard)
Oct 18, 2019 PriorityQueue Sol
>Runtime: 5 ms, faster than 75.88% of Java online submissions for Merge k Sorted Lists.  
>Memory Usage: 41.4 MB, less than 42.08% of Java online submissions for Merge k Sorted Lists.  
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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        } else if (lists.length == 1) {
            return lists[0];
        }
        
        PriorityQueue<ListNode> heap = new PriorityQueue<>(lists.length, new Comparator<ListNode>(){
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val <= o2.val ? -1 : 1;
            }
        });
        
        for (int i = 0; i < lists.length; i++) {
            // lists[i] could be null, like [[]] or [[],[]]
            if (lists[i] != null) {
                heap.offer(lists[i]);
            }
        }
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        while(!heap.isEmpty()) {
            ListNode min = heap.poll();
            prev.next = min;
            if (min.next != null) {
                heap.offer(min.next);
            }
            prev = min;
        }
        
        return dummy.next;
    }
}
```
### [147](https://leetcode.com/problems/insertion-sort-list/) Insertion Sort List (Medium)
Oct 18, 2018
>Runtime: 28 ms, faster than 70.89% of Java online submissions for Insertion Sort List.  
>Memory Usage: 35.9 MB, less than 100.00% of Java online submissions for Insertion Sort List.  
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
    public ListNode insertionSortList(ListNode head) {
        ListNode sorted = null;
        while (head != null) {
            ListNode next = head.next;
            sorted = insert(sorted, head);
            head = next;
        }
        return sorted;
    }
    
    private ListNode insert(ListNode head, ListNode node) {
        node.next = null;
        if (head == null || head.val >= node.val) {
            node.next = head;
            head = node;
            return head;
        }
        ListNode prev = head;
        ListNode cur = head;
        while (cur != null && cur.val < node.val) {
            prev = cur;
            cur = cur.next;
        }
        prev.next = node;
        node.next = cur;
        return head;
    }
}
```
### [148](https://leetcode.com/problems/sort-list/) Sort List (Medium)
Oct 19, 2019
>Runtime: 4 ms, faster than 62.67% of Java online submissions for Sort List.  
>Memory Usage: 40.5 MB, less than 71.93% of Java online submissions for Sort List.  
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
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode secondHalf = cutList(head);
        head = sortList(head);
        secondHalf = sortList(secondHalf);
        return merge(head, secondHalf);
    }
    
    private ListNode cutList(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode next = slow.next;
        slow.next = null;
        return next;
    }
    
    private ListNode merge(ListNode h1, ListNode h2) {
        if (h1 == null || h2 == null) {
            return h1 == null ? h2 : h1;
        }
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while (h1 != null && h2 != null) {
            if (h1.val <= h2.val) {
                cur.next = h1;
                h1 = h1.next;
            } else {
                cur.next = h2;
                h2 = h2.next;
            }
            cur = cur.next;
        }
        cur.next = h1 == null ? h2 : h1;
        return dummy.next;
    }
}
```
### [707](https://leetcode.com/problems/design-linked-list/) Design LinkedList (Medium)
Oct 19, 2019
>Runtime: 51 ms, faster than 84.79% of Java online submissions for Design Linked List.  
>Memory Usage: 45.3 MB, less than 88.89% of Java online submissions for Design Linked List.  
```java
class MyLinkedList {
    private class ListNode{
        int val;
        ListNode prev;
        ListNode next;
        ListNode(int x) {
            val = x;
        }
    }
    
    private ListNode head;
    private ListNode tail;
    private int size;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        head = null;
        tail = null;
        size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode target = null;
        if (index <= size / 2) {
            target = head;
            for (int i = 0; i < index; i++) {
                target = target.next;
            }
        } else {
            target = tail;
            for (int i = size - 1; i > index; i--) {
                target = target.prev;
            }
        }
        return target.val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        ListNode node = new ListNode(val);
        node.next = head;
        if (head != null) {
            head.prev = node;
        }
        head = node;
        size += 1;
        if (tail == null) {
            tail = node;
        }
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        ListNode node = new ListNode(val);
        node.prev = tail;
        if (tail != null) {
            tail.next = node;
        }
        tail = node;
        if (head == null) {
            head = tail;
        }
        size += 1;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index <= 0) {
            addAtHead(val);
            return;
        } else if (index == size) {
            addAtTail(val);
            return;
        } else if (index > size) {
            return;
        }
        ListNode target = head;
        if (index < size / 2) {
            for (int i = 0; i < index; i++) {
                target = target.next;
            }
        } else {
            target = tail;
            for (int i = size - 1; i > index; i--) {
                target = target.prev;
            }
        }
        ListNode node = new ListNode(val);
        node.next = target;
        node.prev = target.prev;
        target.prev.next = node;
        target.prev = node;
        size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index >= 0 && index < size) {
            ListNode target = head;
            if (index < size / 2) {
                for (int i = 0; i < index; i++) {
                    target = target.next;
                }
            } else {
                target = tail;
                for (int i = size - 1; i > index; i--) {
                    target = target.prev;
                }
            }
            if (target != head) {
                target.prev.next = target.next;
            } else {
                head = target.next;
                if (head != null) {
                    head.prev = null;
                }
            }
            if (target != tail) {
                target.next.prev = target.prev;
            } else {
                tail = target.prev;
                if (tail != null) {
                    tail.next = null;
                }
            }
            size--;
        }
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```
  
## Search
DFS, BFS
### [529](https://leetcode.com/problems/minesweeper/). Minesweeper (Medium)
Oct 20, 2019
>Runtime: 1 ms, faster than 85.93% of Java online submissions for Minesweeper.  
>Memory Usage: 38.4 MB, less than 100.00% of Java online submissions for Minesweeper.  
```java
class Solution {
    public char[][] updateBoard(char[][] board, int[] click) {
        if (board[click[0]][click[1]] == 'M') {
            board[click[0]][click[1]] = 'X';
        } else {
            dfs(board, click);
        }
        return board;
    }
    
    private void dfs(char[][] board, int[] click) {
        int x = click[0], y = click[1];
        int n = board.length, m = board[0].length;
        int mines = 0;
        int[] dx = {-1, 0, 1};
        int[] dy = {-1, 0, 1};
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                int nx = x + dx[i];
                int ny = y + dy[j];
                if ( nx >= 0 && nx < n && ny >= 0 && ny < m && !(dx[i] == 0 && dy[j] == 0)) {
                    if (board[nx][ny] == 'M') {
                        mines++;
                    }
                }
            }
        }
        if (mines == 0) {
            board[x][y] = 'B';
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    int nx = x + dx[i];
                    int ny = y + dy[j];
                    if ( nx >= 0 && nx < n && ny >= 0 && ny < m && !(dx[i] == 0 && dy[j] == 0)) {
                        if (board[nx][ny] == 'E') {
                            dfs(board, new int[]{nx, ny});
                        }
                    }
                }
            }
        } else {
            board[x][y] = (char)('0' + mines);
        }
    }
}
```
  
## Greedy

### 218. Skyline Problem  
Oct 14, 2019 扫描线
```java
```

## Two pointers

### [11](https://leetcode.com/problems/container-with-most-water/). Container With Most Water (Medium)
Oct 23, 2019 O(n)
>Runtime: 2 ms, faster than 95.10% of Java online submissions for Container With Most Water.  
>Memory Usage: 39.8 MB, less than 95.51% of Java online submissions for Container With Most Water.  
```java
class Solution {
    public int maxArea(int[] height) {
        int ans = 0;
        int left = 0;
        int right = height.length - 1;
        while (left < right) {
            ans = Math.max(ans, Math.min(height[left], height[right]) * (right - left));
            // move the shorter line
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return ans;
    }
}
```

### [167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/). Two Sum II - Input array is sorted (Easy)
Oct 23, 2019 O(n)
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Two Sum II - Input array is sorted.  
>Memory Usage: 38.1 MB, less than 95.52% of Java online submissions for Two Sum II - Input array is sorted.  
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        while (left < right) {
            if (numbers[left] + numbers[right] == target) {
                return new int[]{left + 1, right + 1};
            } else if (numbers[left] + numbers[right] < target) {
                left++;
            } else {
                right--;
            }
        }
        return new int[]{-1, -1};
    }
}
```

### [977](https://leetcode.com/problems/squares-of-a-sorted-array/). Squares of a Sorted Array (Easy)
Oct 23, 2019
>Runtime: 1 ms, faster than 100.00% of Java online submissions for Squares of a Sorted Array.  
>Memory Usage: 41 MB, less than 95.73% of Java online submissions for Squares of a Sorted Array.  
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int N = A.length;
        int j = 0;
        while (j < N && A[j] < 0) {
            j++;
        } 
        int i = j - 1;
        int[] ans = new int[N];
        int idx = 0;
        while (i >= 0 && j < N) {
            if (-A[i] < A[j]) {
                ans[idx] = A[i] * A[i];
                i--;
            } else {
                ans[idx] = A[j] * A[j];
                j++;
            }
            idx++;
        }
        while (i >= 0) {
            ans[idx] = A[i] * A[i];
            i--;
            idx++;
        }
        while (j < N) {
            ans[idx] = A[j] * A[j];
            j++;
            idx++;
        }
        return ans;
    }
}
```
