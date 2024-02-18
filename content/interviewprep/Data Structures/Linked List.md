---
tags:
  - Algorithm
Linked List:
---
# Linked-Lists
Prefer a Linked List over an [[Array]] when:
- You require very fast insertions or deletions
- You don't need random access to items in the list (you have to start from position 0 and iterate)
- You can't evaluate the exact size of the list (it may grow or shrink during execution)

# Big O
### Time
- $O(1)$ to add or remove an item at the start of the list
- $O(n)$ to add or remove an item at the end of the list
- $O(n)$ to find and access via an index
- $O(1)$ to update
- $O(n)$ to insert or remove elsewhere

# Singly-Linked
Has a nullable reference to the next node in the List:
```java
class ListNode {
	int value;
	ListNode next ;
}
```

# Doubly-Linked
Has nullable references to both the next and previous nodes in the list:
```java
class ListNode {
	int value;
	ListNode previous;
	ListNode next;
}
```

# Delete Kth Node From End
* Use two pointers 
	* Move the first pointer `k` steps away from the head
	* Move both pointer at the same pace until the end of the list
	* Delete the node at the first pointer

```java
/**  
 * delete the N node, N is count from right to left * @param head  
 * @param n  
 * @return  
 */  
 public class DeleteLastN {  
  
    public static void main(String[] args) {  
        ListNode head = init(new int[]{1, 2, 3, 4, 5});  
        // 1,2,3,4,5
        printList(head);  
        head = deleteLastN(head, 2); 
        // 1,2,3,5 
        printList(head);  
    }

	public static ListNode deleteLastN(ListNode head, int n) {  
	    ListNode dummy = new ListNode(0);  
	    dummy.next = head;  
	    ListNode p1 = dummy;  
	    for(int i = 1; i <= n; i++) {  
	        p1 = p1.next;  
	    }  
	  
	    ListNode p2 = dummy;  
	    // locate p2 (the left node of the node to be deleted)  
	    while(p1 != null && p1.next != null) {  
	        p1 = p1.next;  
	        p2 = p2.next;  
	    }  
	  
	    p2.next = p2.next.next;  
	  
	    return dummy.next;  
    }
 }
```

# Reverse Linked List
```java
/**  
 * reverse linked list * @param head  
 * @return  
 */  
public static ListNode reverse(ListNode head) {  
    if(head == null || head.next == null) return head;  
  
    ListNode pre = null;  
    ListNode cur = head;  
    while(cur != null) {  
        ListNode next = cur.next;  
        cur.next = pre;  
        pre = cur;  
        cur = next;  
    }
  
    return pre;  
}
```

# Reverse Linked List from m to n

```java

/**  
 * reverse from node m to n, 1-based index, inclusive m and n */public class ReverseNodeFromM2N {  
  
    public static void main(String[] args) {  
        ListNode head = init(new int[]{1, 2, 3, 4, 5});  
  
        // 1,2,3,4,5  
        printList(head);  
        head = reverseFromM2N(head, 2, 3);  
        // 1,4,5  
        printList(head);  
    }  
  
    /**  
     *     * @param head  
     * @param from: 1-based index  
     * @param to  
     * @return  
     */  
    public static ListNode reverseFromM2N(ListNode head, int from, int to) {  
        ListNode dummy = new ListNode(0);  
        dummy.next = head;  
  
        // pre node before node from  
        ListNode pre = dummy;  
        for(int i = 1; i < from; i++) {  
            pre = pre.next;  
        }  
  
        // reverse nodes between m and n  
        ListNode cur = pre.next;  
        ListNode tail = pre.next;  
        for(int i = from; i <= to; i++) {  
            ListNode next = cur.next;  
            if(i == to) { // update tail node for the last iteration  
               tail.next = next;  
            }  
  
            cur.next = pre.next;  
            pre.next = cur;  
            cur = next;  
        }  
  
        return dummy.next;  
    }  
}

```

# Reverse Linked List by group

```java

/**  
 * K个一组反转链表  
 */  
public class RerverseKGroup {  
  
    public static void main(String[] args) {  
        ListNode head = ListUtil.initList(10);  
        ListUtil.printList(head);  
  
        ListNode newHead = reverseKGroup(head, 2);  
        ListUtil.printList(newHead);  
    }  
  
    /**  
     * K个一组，反转链表head, 递归方式
     * 不足 K 个的分组不反转
     * @param head  
     * @param k  
     * @return  
     */  
    public static ListNode reverseKGroup(ListNode head, int k) {  
        if(head == null || head.next == null) return head;  
  
        int count = 0;  
        ListNode cur = head;  
        while(cur != null && count < k) {  
            ++count;  
            cur = cur.next;  
        }  
        if(count < k) return head;  
  
        // 反转head到cur之间的节点，不包括cur节点  
        ListNode newHead = reverseList(head, cur);  
        // 递归反转下一组开始的节点  
        ListNode newNode = reverseKGroup(cur, k);  
        // 修改两组之间节点指针，第一组结束节点指向第二组开始节点  
        head.setNext(newNode);  
  
        return newHead;  
    }  
  
    /**  
     * 反转head到end之间的链表，不包括end  
     * @param head  
     * @param end  
     * @return  
     */  
    private static ListNode reverseList(ListNode head, ListNode end) {  
        ListNode pre = null, cur = head, next = null;  
        while(cur != end) {  
            next = cur.next;  
            cur.next = pre;  
            pre = cur;  
            cur = next;  
        }  
  
        return pre;  
    }  
}

```
# Palindrome Linked List

```java

public class Palindrome {

    public static void main(String[] args) {  
        ListNode head = new ListNode(1);  
        ListNode n2 = new ListNode(2);  
        ListNode n3 = new ListNode(3);  
        ListNode n4 = new ListNode(2);  
        ListNode n5 = new ListNode(1);  
        head.next = n2;  
        n2.next = n3;  
        n3.next = n4;  
//        n2.next = n4;  
  
        n4.next = n5;  
  
        System.out.println("isPalindrome:" + isPalindrome(head));  
    }

	/**  
	 * reverse the second half, then compare the first and second half，  
	 * @param head  
	 * @return  
	 */  
	public static boolean isPalindrome(ListNode head) {  
	    ListNode fast = head;  
	    ListNode slow = head;  
	    while(fast != null && fast.next != null) {  
	        slow = slow.next;  
	        fast = fast.next.next;  
	    }  
	    if(fast == null) {  
	        slow = slow.next; // odd num of nodes need to advance one step for finding the second half  
	    }  
	  
	    // reverse the second half  
	    ListNode p2 = reverse(slow);  
	    ListNode p1 = head;  
	    // compare the first and the second half to check if it is same  
	    while(p1 != null && p2 != null) {  
	        if(p1.value != p2.value) return false;  
	        p1 = p1.next;  
	        p2 = p2.next;  
	    }  
	  
	    return true;  
	}
}

```

```java
/**
** check palindrome in recursive way, it is difficult to understand but works
**/

public class Palindrome {  
    private static ListNode left = null;

 . public static void main(String[] args) {  
        ListNode head = new ListNode(1);  
        ListNode n2 = new ListNode(2);  
        ListNode n3 = new ListNode(3);  
        ListNode n4 = new ListNode(2);  
        ListNode n5 = new ListNode(1);  
        head.next = n2;  
        n2.next = n3;  
        n3.next = n4;  
//        n2.next = n4;  
  
        n4.next = n5;  
  
        System.out.println("isPalindrome:" + isPalindrome(head));  
    }
    
	public static boolean isPalindrome(ListNode head) {  
	    left = head;  
	    return traverse(head);  
	}  
  
	private static boolean traverse(ListNode right) {  
	    if(right == null) return true;  
	  
	    boolean ans = traverse(right.next);  
	    ans = ans && (right.value == left.value);  
	    left = left.next;  
	    return ans;  
	}
}
```

# List Tools

```java
public class ListUtil {  
    public static void printList(ListNode head) {  
        StringBuilder sb = new StringBuilder();  
        while(head != null) {  
            sb.append(head.value).append(",");  
            head = head.next;  
        }  
        if(sb.length() > 0)  
            sb.deleteCharAt(sb.length() - 1);  
  
        System.out.println(sb.toString());  
    }  
  
    public static ListNode initList(int n) {  
        ListNode head = new ListNode(1);  
        int i = 2;  
        ListNode pre = head;  
        while(i < n) {  
            ListNode node = new ListNode(i);  
            pre.setNext(node);  
            pre = node;  
            ++i;  
        }  
  
        return head;  
    }  
  
    /**  
     * reverse linked list     * @param head  
     * @return  
     */  
    public static ListNode reverse(ListNode head) {  
        if(head == null || head.next == null) return head;  
  
        ListNode pre = null;  
        ListNode cur = head;  
        while(cur != null) {  
            ListNode next = cur.next;  
            cur.next = pre;  
            pre = cur;  
            cur = next;  
        }  
  
        return pre;  
    }  
  
    /**  
     * init Linked List by Array     * @param arr  
     * @return  
     */  
    public static ListNode init(int[] arr) {  
        ListNode dummy = new ListNode(0);  
        ListNode pre = dummy;  
        ListNode cur;  
        for(int i = 0; i < arr.length; i++) {  
            cur = new ListNode(arr[i]);  
            pre.next = cur;  
            pre = cur;  
        }  
  
        return dummy.next;  
    }  
}
```