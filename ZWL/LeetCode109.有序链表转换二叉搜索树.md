# LeetCode109.有序链表转换二叉搜索树
## 题目描述
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点的左右两个子树的高度差的绝对值不超过 1。

## 示例
```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```
## 解法1
先使用快慢指针找到链表的中间点，由于链表为升序，所以该点为树根。随后将左半部分链表的尾端的下一个节点指向Null，右半部分的起始节点为slow.next，递归的建立左子树和右子树，最后return树根。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head==null) return null;
        ListNode fast=head;
        ListNode slow=head;
        ListNode prev=null;
        while (fast.next!=null&&fast.next.next!=null){
            fast=fast.next.next;
            prev=slow;
            slow=slow.next;
        }
        TreeNode root=new TreeNode(slow.val);
        if (prev!=null){
            prev.next=null;
            root.left=sortedListToBST(head);
        }
        root.right=sortedListToBST(slow.next);
        return root;
    }
}
```
**时间复杂度：O(NlogN)，空间复杂度：O(logN)，递归需要考虑栈空间复杂度**


## 解法2（优化）
模拟二叉树的中序遍历，二叉搜索树的中序遍历为升序，所以这样能够按照从左往右的顺序建立树节点，所以时间复杂度降低为O(N)。
```java
/**
 * Definition for singly-linked list. public class ListNode { int val; ListNode next; ListNode(int
 * x) { val = x; } }
 */
/**
 * Definition for a binary tree node. public class TreeNode { int val; TreeNode left; TreeNode
 * right; TreeNode(int x) { val = x; } }
 */
class Solution {

  private ListNode head;

  private int findSize(ListNode head) {
    ListNode ptr = head;
    int c = 0;
    while (ptr != null) {
      ptr = ptr.next;  
      c += 1;
    }
    return c;
  }

  private TreeNode convertListToBST(int l, int r) {
    // Invalid case
    if (l > r) {
      return null;
    }

    int mid = (l + r) / 2;

    // First step of simulated inorder traversal. Recursively form
    // the left half
    TreeNode left = this.convertListToBST(l, mid - 1);

    // Once left half is traversed, process the current node
    TreeNode node = new TreeNode(this.head.val);
    node.left = left;

    // Maintain the invariance mentioned in the algorithm
    this.head = this.head.next;

    // Recurse on the right hand side and form BST out of them
    node.right = this.convertListToBST(mid + 1, r);
    return node;
  }

  public TreeNode sortedListToBST(ListNode head) {
    // Get the size of the linked list first
    int size = this.findSize(head);

    this.head = head;

    // Form the BST now that we know the size
    return convertListToBST(0, size - 1);
  }
}
```
注：部分转载自LeetCode官方题解
