# Struggling for LeetCode (Java Solution)  
  
>来个Offer吧！秋梨膏！

## Binary Search
[low, high)  
lowerBound  
upperBound  
### [4](https://leetcode.com/problems/median-of-two-sorted-arrays/). Median of Two Sorted Arrays (hard)
Nov 3, 2019
>Runtime: 2 ms, faster than 99.97% of Java online submissions for Median of Two Sorted Arrays.  
>Memory Usage: 46.4 MB, less than 91.67% of Java online submissions for Median of Two Sorted Arrays.  
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        if (n1 == 0 && n2 == 0) {
            return 0;
        }
        if (n1 == 0 || n2 == 0) {
            return n1 == 0 ? findMedianSortedArray(nums2) : findMedianSortedArray(nums1);
        }
        // deal with the shorter array
        if (n1 > n2) {
            return findMedianSortedArrays(nums2, nums1);
        }
        int n = n1 + n2;
        int m = (n + 1) / 2;
        // contain m1 elements from arr1, m2 elements from arr 2
        // m1 + m2 == m
        int m1 = 0, m2 = 0;
        // binary search to partition
        int left = 0; int right = n1;
        while (left < right) {
            // 0 x 1 x ... x n1
            m1 = (right - left) / 2 + left;
            m2 = m - m1;
            // stop condition: nums1[m1] > nums2[m2 - 1] && nums1[m1 - 1] < nums2[m2]
            if (nums1[m1] < nums2[m2 - 1]) {
                // should include more elements in arr1
                left = m1 + 1;
            } else {
                right = m1;
            }
        }
        m1 = left;
        m2 = m - m1;
        int c1 = m1 == 0 ? Integer.MIN_VALUE : nums1[m1 - 1];
        int c2 = m2 == 0 ? Integer.MIN_VALUE : nums2[m2 - 1];
        if (n % 2 == 1) {
            return Math.max(c1, c2);
        } else {
            int c3 = m1 == n1 ? Integer.MAX_VALUE : nums1[m1];
            int c4 = m2 == n2 ? Integer.MAX_VALUE : nums2[m2];
            return (Math.max(c1, c2) + Math.min(c3, c4)) / 2.0;
        }
    }
    
    public double findMedianSortedArray(int[] nums) {
        if (nums.length % 2 == 1) {
            return nums[nums.length / 2];
        } else {
            return (nums[nums.length / 2] + nums[nums.length / 2 - 1]) / 2.0;
        }
    }
}
```
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
  
## Tree
### [255](https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree/). Verify Preorder (Medium)
Oct 26, 2019
>Runtime: 2 ms, faster than 96.57% of Java online submissions for Verify Preorder Sequence in Binary Search Tree.  
>Memory Usage: 39.1 MB, less than 100.00% of Java online submissions for Verify Preorder Sequence in Binary Search Tree.  
```java
class Solution {
    private int idx = 0;
    int[] tree = null;
    public boolean verifyPreorder(int[] preorder) {
        idx = 0;
        tree = preorder;
        return verifyPreorder(Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
    
    private boolean verifyPreorder(int min, int max) {
        if (idx == tree.length) {
            return true;
        }
        int root = tree[idx];
        if (root < min || root > max) {
            return false;
        }
        idx++;
        return verifyPreorder(min, root - 1) || verifyPreorder(root + 1, max);
    }
}
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

## Recursion

### [856](https://leetcode.com/problems/score-of-parentheses/). Score of Parentheses (Medium)
Oct 23, 2019 O(n^2)
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Score of Parentheses.  
>Memory Usage: 33.9 MB, less than 100.00% of Java online submissions for Score of Parentheses.  
```java
class Solution {
    public int scoreOfParentheses(String S) {
        if (S == null || S.length() == 0) {
            return 0;
        }
        return helper(S, 0, S.length() - 1);
    }
    
    private int helper(String S, int l, int r) {
        if (l >= r) {
            return 0;
        }
        if (r - l == 1) {
            return 1;
        }
        int count = 0;
        for (int i = l; i < r; i++) {
            if (S.charAt(i) == '(') {
                count++;
            } else if (S.charAt(i) == ')') {
                count--;
            }
            if (count == 0) {
                return helper(S, l, i) + helper(S, i + 1, r);
            }
        }
        return 2 * helper(S, l + 1, r - 1);
    }
}
```
### [726](https://leetcode.com/problems/number-of-atoms/). Number of Atoms (hard)
Oct 25, 2019 O(n)
>Runtime: 4 ms, faster than 74.44% of Java online submissions for Number of Atoms.  
>Memory Usage: 34.7 MB, less than 100.00% of Java online submissions for Number of Atoms.  
```java
class Solution {
    private int idx = 0;
    public String countOfAtoms(String formula) {
        StringBuilder sb = new StringBuilder();
        idx = 0;
        Map<String, Integer> map = countOfAtoms(formula.toCharArray());
        for (String name : map.keySet()) {
            sb.append(name);
            int count = map.get(name);
            if (count > 1) {
                sb.append(count);
            }
        }
        return sb.toString();
    }
    
    private Map<String, Integer> countOfAtoms(char[] arr) {
        Map<String, Integer> map = new TreeMap<>();
        while (idx < arr.length) {
            if (arr[idx] == '(') {
                idx++;
                // recursion
                Map<String, Integer> tmp = countOfAtoms(arr);
                int factor = getNumber(arr);
                for (Map.Entry<String, Integer> entry : tmp.entrySet()) {
                    Integer count = map.get(entry.getKey());
                    if (count == null) {
                        count = 0;
                    }
                    map.put(entry.getKey(), count + factor * entry.getValue());
                }
            } else if (arr[idx] == ')') {
                idx++;
                return map;
            } else {
                String name = getName(arr);
                map.put(name, map.getOrDefault(name, 0) + getNumber(arr));
            }
        }
        return map;
    }
    
    private String getName(char[] arr) {
        String name = "" + arr[idx];
        idx++;
        while (idx < arr.length && arr[idx] >= 'a' && arr[idx] <= 'z') {
            name += arr[idx];
            idx++;
        }
        return name;
    }
    
    private int getNumber(char[] arr) {
        int res = 0;
        while (idx < arr.length && arr[idx] >= '0' && arr[idx] <= '9') {
            res = res * 10 + arr[idx] - '0';
            idx++;
        }
        return res == 0 ? 1 : res;
    }
}
```
### [394](https://leetcode.com/problems/decode-string/). Decode String (Medium)
Oct 25, 2019
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Decode String.  
>Memory Usage: 34.3 MB, less than 100.00% of Java online submissions for Decode String.  
```java
class Solution {
    private int idx = 0;
    public String decodeString(String s) {
        idx = 0;
        return decodeSubString(s.toCharArray());
    }
    
    private String decodeSubString(char[] arr) {
        StringBuilder sb = new StringBuilder();
        int factor = 1;
        String tmp = null;
        while (idx < arr.length) {
            if (arr[idx] >= '0' && arr[idx] <= '9') {
                factor = getNumber(arr);
            } else if (arr[idx] == '[') {
                idx++;
                tmp = decodeSubString(arr);
                for (int i = 0; i < factor; i++) {
                    sb.append(tmp);
                }
            } else if (arr[idx] == ']') {
                idx++;
                return sb.toString();
            } else {
                sb.append(arr[idx]);
                idx++;
            }
        }
        return sb.toString();
    }
    
    private int getNumber(char[] arr) {
        int num = 0;
        while (idx < arr.length && arr[idx] >= '0' && arr[idx] <= '9') {
            num = num * 10 + arr[idx] - '0';
            idx++;
        }
        return num;
    }
}
```
### [736](https://leetcode.com/problems/parse-lisp-expression/) Parse Lisp Expression (hard)
Oct 25, 2019
>
```java

```

## Divide and Conquer
### [169](https://leetcode.com/problems/majority-element/) Majority Element (Easy)
Oct 25, 2019 This problem has multiple solutions
Divide and Conquer - O(nlgn) ~ O(n)
>Runtime: 1 ms, faster than 99.90% of Java online submissions for Majority Element.  
>Memory Usage: 41.8 MB, less than 68.38% of Java online submissions for Majority Element.  
```java
class Solution {
    public int majorityElement(int[] nums) {
        // divide & conquer sol
        return majorityElement(nums, 0, nums.length - 1);
    }
    
    private int majorityElement(int[] nums, int left, int right) {
        // base case
        if (left == right) {
            return nums[left];
        }
        // recursive rule
        int mid = (right - left) / 2 + left;
        int leftNum = majorityElement(nums, left, mid);
        int rightNum = majorityElement(nums, mid + 1, right);
        
        // both agree
        if (leftNum == rightNum) {
            return leftNum;
        }
        
        // return winner
        int leftCount = countInRange(nums, leftNum, left, right);
        int rightCount = countInRange(nums, rightNum, left, right);
        
        return leftCount > rightCount ? leftNum : rightNum;
    }
    
    private int countInRange(int[] nums, int target, int l, int r) {
        int count = 0;
        for (int i = l; i <= r; i++) {
            if (nums[i] == target) {
                count++;
            }
        }
        return count;
    }
}
```
### [315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/). Count of Smaller Numbers After Self (hard)
Oct 25, 2019
>
```java

```
  
## Stack
4道单调栈的应用 Nov 1, 2019
### [1130](). Minimum Cost Tree From Leaf Values (Medium)
Oct 29, 2019 O(n) [REF](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/discuss/339959/One-Pass-O(N)-Time-and-Space)
>Runtime: 1 ms, faster than 96.39% of Java online submissions for Minimum Cost Tree From Leaf Values.  
>Memory Usage: 34.5 MB, less than 100.00% of Java online submissions for Minimum Cost Tree From Leaf Values.  
```java
class Solution {
    public int mctFromLeafValues(int[] arr) {
        int res = 0;
        Deque<Integer> stack = new LinkedList<>();
        stack.offerLast(Integer.MAX_VALUE);
        for (int e : arr) {
            while (e > stack.peekLast()) {
                int smaller = stack.pollLast();
                res += smaller * Math.min(stack.peekLast(), e);
            }
            stack.offerLast(e);
        }
        while (stack.size() > 2) {
            res += stack.pollLast() * stack.peekLast();
        }
        
        return res;
    }
}
```
### [496](https://leetcode.com/problems/next-greater-element-i/). Next Greater Element I (Easy)
>Runtime: 3 ms, faster than 79.40% of Java online submissions for Next Greater Element I.  
>Memory Usage: 37 MB, less than 100.00% of Java online submissions for Next Greater Element I.  
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        Deque<Integer> stack = new LinkedList<>();
        int[] res = new int[nums1.length];
        for (int num : nums2) {
            while (!stack.isEmpty() && num > stack.peekLast()) {
                map.put(stack.pollLast(), num);
            }
            stack.offerLast(num);
        }
        while (!stack.isEmpty()) {
            map.put(stack.pollLast(), -1);
        }
        for (int i = 0; i < res.length; i++) {
            res[i] = map.get(nums1[i]);
        }
        return res;
    }
}
```
### [503](https://leetcode.com/problems/next-greater-element-ii/). Next Greater Element II (Medium)
>Runtime: 11 ms, faster than 87.71% of Java online submissions for Next Greater Element II.  
>Memory Usage: 40.1 MB, less than 100.00% of Java online submissions for Next Greater Element II.  
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Deque<Integer> stack = new LinkedList<>();
        int n = nums.length;
        int[] res = new int[n];
        Arrays.fill(res, -1);
        
        for (int i = 0; i < n * 2; i++) {
            while (!stack.isEmpty() && nums[i % n] > nums[stack.peekLast()]) {
                res[stack.pollLast()] = nums[i % n]; 
            }
            stack.offer(i % n);
        }
        return res;
    }
}
```
### [1019](https://leetcode.com/problems/next-greater-node-in-linked-list/). Next Greater Node In Linked List (Medium)
>Runtime: 26 ms, faster than 84.92% of Java online submissions for Next Greater Node In Linked List.  
>Memory Usage: 40.2 MB, less than 97.30% of Java online submissions for Next Greater Node In Linked List.  
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
    public int[] nextLargerNodes(ListNode head) {
        Map<ListNode, Integer> map = new HashMap<>();
        Deque<ListNode> stack = new LinkedList<>();
        ListNode dummyHead = head;
        int length = 0;
        
        while (head != null) {
            length++;
            while (!stack.isEmpty() && head.val > stack.peekLast().val) {
                map.put(stack.pollLast(), head.val);
            }
            stack.offerLast(head);
            head = head.next;
        }
        
        int[] res = new int[length];
        head = dummyHead;
        for (int i = 0; i < length; i++) {
            Integer value = map.get(head);
            if (value != null) {
                res[i] = value;
            }
            head = head.next;
        }
        return res;
    }
}
```
## String
### [556](https://leetcode.com/problems/next-greater-element-iii/). Next Greater Element III (Medium) 
Nov. 22, 2019
>Runtime: 0 ms, faster than 100.00% of Java online submissions for Next Greater Element III.  
>Memory Usage: 32.9 MB, less than 10.00% of Java online submissions for Next Greater Element III.  
```java
package leetcode;

import java.util.Arrays;

public class NextGreaterElement {
	
    public int nextGreaterElementIII(int n) {
        char[] arr = (n + "").toCharArray();
        // step 1: start from right, find first digit smaller than its right
        int i = arr.length - 2;
        while (i >= 0) {
            if (arr[i] < arr[i + 1]) {
                break;
            }
            i--;
        }
        if (i < 0)
            return -1;
        // step 2: start from i, find minimum digit greater than arr[i]
        int j = i + 1;
        while (j + 1 < arr.length) {
            if (arr[j] > arr[i] && arr[j + 1] <= arr[i]) 
                break;
            j++;
        }
        swap(arr, i, j);
        // step 3 from i + 1 to the end, sort in increasing order
        // reverse(arr, i + 1, j - 1);  // not reverse !
        Arrays.sort(arr, i + 1, arr.length);
        // be careful of overflow !
        long val = Long.parseLong(new String(arr));
        return val > Integer.MAX_VALUE ? -1 : (int)val;
    }
    
    private void swap(char[] arr, int i, int j) {
        char temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    public static void testIII(NextGreaterElement sol, int test) {
    	System.out.println(test);
		System.out.println(sol.nextGreaterElementIII(test));
		System.out.println();
    }

	public static void main(String[] args) {
		NextGreaterElement solution = new NextGreaterElement();
		testIII(solution, 12443322);
		testIII(solution, 1999999999);
	}

}

```
