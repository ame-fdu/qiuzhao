```
class Solution {
    //设立两个节点，一个头节点一个前置节点
    Node head,pre;

    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        //中序遍历
        inOrder(root);
        //最后还要形成循环链表
        pre.right = head;
        head.left = pre;
        return head;

    }

    private void inOrder(Node cur) {
        if(cur == null) return;
        inOrder(cur.left);
        //第一个遍历到的点是头节点
        if(pre == null) head = cur;
        //否则pre的下个节点是cur，cur的上个节点是pre
        else pre.right = cur;//注意节点为空不能有下个节点
        cur.left = pre;
        //令pre=cur继续遍历
        pre = cur;
        inOrder(cur.right);
    }
}
```



# 一、栈

### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

#### A.思路：遇到左括号加入栈，遇到右括号，弹出判断

#### B.代码及注释：

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            //遇到左括号压入栈
            if(c == '(' ||c=='['||c=='{') stack.push(c);
            //遇到右括号弹出栈
            else {
                //没有弹出的false
                if(stack.isEmpty()) return false;
                //不匹配false
                if(c == ')' && stack.pop()!='(') return false;
                if(c == ']' && stack.pop()!='[') return false;
                if(c == '}' && stack.pop()!='{') return false;
            }
        }
        //返回还要确保栈为空
        return stack.isEmpty();
    }
}
```

------



​	

### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

#### A.思路：栈1直接装入新元素，要查看或者pop元素时，操作栈2，为空则倒空栈1到栈2。

#### B.代码及注释：

```java
class MyQueue {
    
    Stack<Integer> stack1;
    Stack<Integer> stack2;
    
    /** Initialize your data structure here. */
    //构造函数，初始化两个栈
    public MyQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    //新入元素直接入栈1
    public void push(int x) {
        stack1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    //pop操作栈2，栈2为空，则将栈1全倒入栈2
    public int pop() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
    
    /** Get the front element. */
    //peek操作栈2，栈2为空，则将栈1全倒入栈2
    public int peek() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();
    }
    
    /** Returns whether the queue is empty. */
    //双栈为空
    public boolean empty() {
        return stack2.isEmpty() &&stack1.isEmpty();
    }
}
```

------

### [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

#### A.思路：队列1直接装入新元素，队列2倒入队列1，然后交换，每次都保持队列1为空

#### B.代码及注释：

```java
class MyStack {
    Queue<Integer> queue1;
    Queue<Integer> queue2;

    /** Initialize your data structure here. */
    public MyStack() {
        queue1 = new LinkedList<>();
        queue2 = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue1.add(x);
        while (!queue2.isEmpty()){
            queue1.add(queue2.remove());
        }
        Queue<Integer> temp = queue1;
        queue1 = queue2;
        queue2 = temp;
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue2.remove();
    }
    
    /** Get the top element. */
    public int top() {
        return queue2.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue2.isEmpty();
    }
}
```



------





### [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

#### A.思路：栈用于翻转单词，word存入单词，res弹出pop然后加入

#### B.代码及注释：

```java
class Solution {
    public String reverseWords(String s) {
        //利用栈翻转字符
        Stack<String> stack = new Stack<>();
        int len = s.length();
        //利用StringBuffer存储每个单词
        StringBuffer word = new StringBuffer();
        for (int i = 0; i < len; i++) {
            if (s.charAt(i) != ' ') {
                word.append(s.charAt(i));
                if (i == len - 1 ||s.charAt(i+1)  == ' ') {
                    //单词进入栈
                    stack.push(word.toString());
                    //重置word
                    word = new StringBuffer();
                }
            }
        }
        //结果集，从stack中pop
        StringBuffer res = new StringBuffer();
        while (!stack.isEmpty()) {
            res.append(stack.pop());
            if (!stack.isEmpty()) {
                res.append(' ');
            }
        }
        return res.toString();
    }
}
```

------





# 二、链表

### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

#### A.思路： a,b上学时间都在9:00 - 9：30之间，问两者上学的间隔时间在15分钟之上的概率

#### B.代码及注释：

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre =dummy;
        ListNode cur =head;
        while (cur!=null&&cur.next!=null){
            ListNode next = cur.next;
            cur.next = cur.next.next;
            next.next = cur;
            pre.next = next;
            pre = cur;
            cur = cur.next;
        }
        return dummy.next;
    }
}
```



------



### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)



#### A.思路：双指针分别出发，结束走对方的路，相遇在交点

#### B.代码及注释：

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null &&headB == null) return null;
        //双指针分别出发
        ListNode A = headA,B = headB;
      	
        //没有相遇一直走
        while (A != B ) {
            //走完换对方的路
            if(A!=null) A = A.next;
            else A = headB;
            if(B!=null) B =B.next;
            else B = headA;
        }
        //相遇在交点
        return A;
    }
}
```

------

### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

#### A.思路：快慢指针，存在环总会相遇

#### B.代码及注释

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        //定义快慢指针
        ListNode fast = head,slow = head;

        while (fast !=null && fast.next!=null){
            fast = fast.next.next;
            slow = slow.next;
            //总会相遇
            if(slow == fast) return true;
        }
        return false;
    }
}
```

------

### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

#### A.思路：快慢指针，存在环总会相遇

#### B.代码及注释：

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        //快慢指针判断是否相遇是否有环
        ListNode fast = head,slow = head;
        while (fast !=null && fast.next!=null){
            fast = fast.next.next;
            slow = slow.next;
            if(slow == fast) break;
        }
        //循环结束，如果fast在末尾说明，无环
        if(fast ==null || fast.next ==null)  return null;
        //若有环，令一个指针重新出发，最终相遇在节点
        fast = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
}
```

------

### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

#### A.思路：找到左中点，反转后半部分

#### B.代码及注释：

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        //找到左中点
        while (fast.next != null && fast.next.next!=null){
            fast = fast.next.next;
            slow = slow.next;
        }
        //反转后半部分
        ListNode after = reverse(slow.next);
        //然后开始注意对比
        while (after != null){
            if(head.val != after.val){
                return false;
            }
            head = head.next;
            after = after.next;
        }
        return true;
    }

    private ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur!=null){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

------



### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

#### A.思路：设置一个pre，cur用于反转，next用于保存下一个节点（迭代）；反转后续链表，反转当前节点（递归）

#### B.代码及注释：

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        /*
        //迭代，设置pre和cur
        ListNode pre=null;
        ListNode cur=head;
        while (cur!=null){
            //保存下一个节点
            ListNode temp = cur.next;
            //反转
            cur.next = pre;
            //向后移动更新
            pre = cur;
            cur = temp;
        }
        return pre;*/

        //递归终止条件，为空
        if(head == null||head.next == null) return head;
        //递归反转后一个节点
        ListNode cur = reverseList(head.next);
        //子问题：只反转当前节点
        head.next.next = head;
        head.next = null;
        return cur;
    }
}
```

------

### [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

#### A.思路：先找到反转链表pre，及自己部分[l,r]，next，先断开，再反转l，再相连

#### B.代码及注释：

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        //虚拟头节点头节点会改变
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        //找到反转部分的pre节点
        for (int i = 0; i < left - 1; i++) {
            pre = pre.next;
        }
        //反转部分的头节点
        ListNode l = pre.next;
        //找到反转链表的尾节点
        ListNode r = l;
        for (int i = 0; i < right - left ; i++) {
            r = r.next;
        }
        //保存下一个节点
        ListNode next = r.next;
        //先断开
        pre.next = null;
        r.next = null;
        //再反转
        reverse(l);
        //在连接
        pre.next = r;
        l.next =next;
        return dummy.next;
    }

    private void reverse(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;

        while(cur!=null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
    }
}
```

------



### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

#### A.思路：虚拟头节点，循环反转链表，设置一个pre，一个end每次移动k表示需要反转尾部，结束时end.next=null,非正常结束时，让end移动到null判断并break。正常反转设置start = pre.next，保存next = end.next.断开尾部end.next = null,正常反转后拼接，start.next=next;

#### B.代码及注释：

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        //初始化dummy，需要改变头节点的
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        //用pre和end定位每组k个链表
        ListNode pre = dummy;
        ListNode end = dummy;

        //最后一组k。正常结束时end.next=null
        while (end.next != null){
            //end移动k个，非正常结束时，end=null
            for (int i = 0; i < k && end != null; i++) {
                end = end.next;
            }
            //不满k个提前终止
            if(end == null) break;
            //找到需要反转的链表头节点start
            ListNode start = pre.next;
            //保存next
            ListNode next = end.next;
            //断开并反转
            end.next = null;
            pre.next = reverse(start);
            //重新链接
            start.next = next;
            //第二次循环pre为start，end为start
            pre =start;
            end = start;
        }
        return dummy.next;
    }

    //反转链表
    private ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur!=null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```

------

### [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

#### A.思路：分别设立奇odd节点头，evenHead偶节点头

#### B.代码及注释：

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null){
            return head;
        }
        //偶节点头
        ListNode evenHead = head.next;
        //奇偶指针
        ListNode odd = head,even = evenHead;
        while (even!=null&&even.next!=null){
            //连接奇链表
            odd.next = even.next;
            odd = odd.next;
            //连接偶链表
            even.next =odd.next;
            even = even.next;
        }
        //连接奇偶链表
        odd.next = evenHead;
        return head;
    }
}
```



------



### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

#### A.思路：虚拟头节点处理头更新问题

#### B.代码及注释：

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //虚拟头节点，处理头更新问题
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        //依次比较合并
        while (l1!=null&&l2!=null){
            if(l1.val <= l2.val){
              	//合并l1
                cur.next = l1;
                l1 = l1.next;
            }else {
              	//合并l2
                cur.next = l2;
                l2 = l2.next;
            }
            //移动cur
            cur = cur.next;
        }
        //合并掉剩下的
        cur.next = l1==null? l2:l1;
        //虚拟头节点的返回方法
        return dummy.next;
    }
}
```

------

### [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

#### A.思路：子问题看作合并两个链表，递归使用归并法

#### B.代码及注释：

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        //使用归并法合并
        return merge(lists,0,lists.length-1);
    }


    private ListNode merge(ListNode[] lists, int l, int r) {
        //递归终止条件
        if(l==r) return lists[l];
        if(l>r) return null;
        //二分法
        int m = l+(r-l)/2;
        //递归合成左右链表
        return mergeTwoLists(merge(lists,l,m),merge(lists,m+1,r));
    }
    //合并两个链表
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while (l1!=null&&l2!=null){
            if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
                cur = cur.next;
            }else {
                cur.next = l2;
                l2 = l2.next;
                cur = cur.next;
            }
        }
        cur.next = l1==null? l2:l1;
        return dummy.next;
    }
}
```

------

### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

#### A.思路：快慢指针，快先走k步

#### B.代码及注释：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode slow = head,fast = head;
        for (int i = 0; i < k; i++) {
            fast = fast.next;
        }
        while (fast!=null){
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

------

### [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

#### A.思路:快慢指针，快的每次走两步(中间右节点终止条件为fast.next!=null)

#### B.代码及注释:



```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head,fast = head;
        //返回右边中节点
        while (fast!=null&&fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```

------

### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

#### A.思路:找左中点+断开+反转+合并

#### B.代码及注释:

```java
class Solution {
    public void reorderList(ListNode head) {
        //快慢指针找到左中点
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next !=null &&fast.next.next!=null){
            slow = slow.next;
            fast = fast.next.next;
        }
        //右边链表为l2
        ListNode l2 = slow.next;
        //断开左边
        slow.next = null;
        //反转右边
        l2 = reverse(l2);
        //左边为l1
        ListNode l1= head;
        //合并l1,l2，注意从最后一个连接往前断开
        while (l2!=null){
            ListNode temp = l2.next;
            l2.next = l1.next;
            l1.next = l2;
            l1 = l2.next;
            l2 = temp;
        }
    }
    //反转链表
    private ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur!=null){
            ListNode next = cur.next;
            cur.next =pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```

------



### [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

#### A.思路:先找到倒数第N各节点，因为可能删除头节点设置虚拟头节点

#### B.代码及注释：

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        //虚拟头节点
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode slow = dummy,fast = dummy;
        //fast先移动n+1；
        for (int i = 0; i < n+1; i++) {
            fast = fast.next;
        }
        //slow找到该节点前节点
        while (fast!=null){
            slow = slow.next;
            fast = fast.next;
        }
        //删除该节点
        slow.next  = slow.next.next;
        return dummy.next;

    }
}
```

------

### [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

#### A.思路:判断cur.val与cur.next.val，cur.next = cur.next.next;

#### B.代码及注释：

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null){
            //有重复节点时，跳过该节点，依次检查
            if(cur.val == cur.next.val){
                cur.next = cur.next.next;
            }else {
                cur = cur.next;
            }
        }
        return head;
    }
}
```

------

### [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

#### A.思路:存在处理头节点问题，设置虚拟头节点,需要pre，并在出现重复时cur = cur.next排重

#### B.代码及注释：

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode cur = head;
        while (cur!= null && cur.next!=null) {
            //当出现重复节点
            if(cur.val == cur.next.val){
                //将cur指向重复值最后一个
                while (cur.next!=null &&cur.val == cur.next.val) cur = cur.next;
                pre.next = cur.next;
                cur =cur.next;
            }else {
                pre = pre.next;
                cur = cur.next;
            }
        }
        return dummy.next;
    }
}
```

------



# 三、二叉树

### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

#### A.思路：辅助队列，用size区分每层存储在level中。

#### B.代码及注释：

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //初始化结果
        List<List<Integer>> res = new ArrayList<>();
        //层序遍历辅助队列，add、remove、peek。
        Queue<TreeNode> queue = new LinkedList<>();
        //先加入根节点
        if(root != null) queue.add(root);
        //队列非空循环
        while (!queue.isEmpty()){
            //该层的节点数
            int n = queue.size();
            //level存储每层的节点
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                //每拿出一个节点加入level的同时，加入其左右子节点
                TreeNode node = queue.remove();
                level.add(node.val);
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            //一层结束，加入res
            res.add(level);
        }
        return res;
    }
}
```

------

### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

#### A.思路：层序遍历，每次输出最后一个即可

#### B.代码及注释：

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        //层序遍历辅助队列
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            //每层size
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.remove();
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
                //如果是最后一个则加入res
                if(i == size-1){
                    res.add(node.val);
                }
            }
        }
        return res;
    }
}
```

------

### [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

#### A.思路：层序遍历，每次用一个flag判断加头还是加尾，LinkedList

#### B.代码及注释：

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        //利用双端队列实现两边加
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) return res;
        //层序遍历利用辅助队列的标准写法
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        //设置一个flag判断从前加还是从后加
        boolean flag =true;
        while (!queue.isEmpty()){
            Deque<Integer> level = new LinkedList<>();
            int n = queue.size();
            for (int i = 0; i < n; i++) {
                TreeNode node = queue.remove();
                if(flag){
                    level.addLast(node.val);
                }else {
                    level.addFirst(node.val);
                }

                if(node.left != null) queue.add(node.left);
                if(node.right!= null) queue.add(node.right);
            }
            res.add(new LinkedList<Integer>(level));
            //反转flag
            flag = !flag;
        }
        return res;
    }
}
```

------



### [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

#### A.思路：层序遍历，每次处理为node指向peek

#### B.代码及注释：

```java
class Solution {
    public Node connect(Node root) {
        Queue<Node> queue = new LinkedList<>();
        if(root!=null) queue.add(root);
        while (!queue.isEmpty()){
            int n = queue.size();
            for (int i = 0; i < n; i++) {
                Node node = queue.remove();
                if(i==n-1) node.next = null;
                else node.next = queue.peek();
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
        }
        return root;
    }
}
```

------

### [662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

#### A.思路：层序遍历，利用一个list记录index

#### B.代码及注释：

```java
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
      	//Queue存Node
        Queue<TreeNode> queue  = new LinkedList<>();
      	//用一个LinkedList存坐标
        LinkedList<Integer> list = new LinkedList<>();
      	//先放入头和坐标
        queue.add(root);
        list.add(1);
        int res = 1;
      	//循环判断
        while (!queue.isEmpty()){
            int count = queue.size();
            for (int i = 0; i < count; i++) {
                TreeNode cur = queue.remove();
              	//每次removeFirst该点的坐标
                Integer curIndex = list.removeFirst();
              	//左不为空，存节点入queue，存坐标curIndex*2入list
                if(cur.left != null){
                    queue.add(cur.left);
                    list.add(curIndex*2);
                }
              	//右同理
                if(cur.right != null){
                    queue.add(cur.right);
                    list.add(curIndex*2+1);
                }
            }
          	
            if(list.size() >= 2){
                res = Math.max(res,list.getLast() - list.getFirst() + 1);
            }
        }
        return res;
    }
}
```

------



### [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)

#### A.思路：层序遍历，空节点也加入队列，但第二次碰到直接改变flag，continue。根据flag判断是否有非空节点

#### B.代码及注释：

```java
class Solution {
    public boolean isCompleteTree(TreeNode root) {
        //BFS 广度优先搜索即层序遍历
        Queue<TreeNode> queue=new LinkedList<>();
        queue.add(root);
        boolean flag = false;
        while (!queue.isEmpty()){
            TreeNode node = queue.remove();
            //当碰到空节点时把flag变为true
            if(node == null){
                flag = true;
                //用continue跳过下面步骤
                continue;
            }
            //空节点之后还有非空节点节点，return
            if(flag) return false;
            queue.add(node.left);
            queue.add(node.right);
        }
        //否则为真
        return true;
    }
}
```

------



### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

#### A.思路：左中右，迭代辅助栈，与前后遍历不同，while循环一直加入左孩子到空为止，然后处理，然后为右

#### B.代码及注释：

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        //辅助栈
        Stack<TreeNode> stack = new Stack<>();
        //直接读node放入栈
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            //把所有左孩子放入栈后
            if (node != null) {
                stack.push(node);
                node = node.left;
            } else {
                //pop+add添加，后开始处理右孩子
                node = stack.pop();
                result.add(node.val);
                node = node.right;
            }
        }
        return result;
    }
}
```

------

### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

#### A.思路：中序遍历后判断

#### B.代码及注释：

```java
class Solution {
    List<Integer> res = new LinkedList<>();
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        inOrder(root);
        for (int i = 1; i < res.size(); i++) {
            if(res.get(i) <= res.get(i-1)) return false;
        }
        return true;
    }

    private void inOrder(TreeNode root) {
        if(root == null) return;
        inOrder(root.left);
        res.add(root.val);
        inOrder(root.right);
    }
}
```

------

### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

#### A.思路：中序遍历，中间处理过程令pre.right = cur，cur.left = pre;并且更新pre = cur；

#### B.代码及注释：

```java
class Solution {
    //设立两个节点，一个头节点一个前置节点
    Node head,pre;

    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        //中序遍历
        inOrder(root);
        //最后还要形成循环链表
        pre.right = head;
        head.left = pre;
        return head;

    }

    private void inOrder(Node cur) {
        if(cur == null) return;
        inOrder(cur.left);
        //第一个遍历到的点是头节点
        if(pre == null) head = cur;
        //否则pre的下个节点是cur，cur的上个节点是pre
        else pre.right = cur;//注意节点为空不能有下个节点
        cur.left = pre;
        //令pre=cur继续遍历
        pre = cur;
        inOrder(cur.right);
    }
}
```

------





### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

#### A.思路：中左右，迭代辅助栈，与层序类似，先入右再入左

#### B.代码及注释：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        //结果集
        List<Integer> res = new ArrayList<>();
        //辅助栈
        Stack<TreeNode> stack = new Stack<>();
        if(root == null) return res;
        //层序遍历类似写法
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            res.add(node.val);
            //先入右再入左
            if(node.right!=null) stack.push(node.right);
            if(node.left!=null) stack.push(node.left);
        }
        return res;
    }
}
```

------

### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

#### A.思路：栈1先遍历左节点再遍历右节点，后序是左右中，由于每次都先碰到中，需要利用栈2反转，迭代辅助栈，与层类似，先入左再入右，但是反转结果，使用栈

#### B.代码及注释：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if (root == null) return res;
        Stack<TreeNode> stack1 = new Stack<TreeNode>();
        //反转栈2，将左右中反转为中右左，即入栈2的顺序
        Stack<Integer> stack2 = new Stack<Integer>();
      
        stack1.push(root);
        while (!stack1.isEmpty()) {
            TreeNode node = stack1.pop();
          	//中先放在栈2
            stack2.push(node.val);
          	//栈1先放左，再放右，弹出来遍历为右左
            if (node.left != null) stack1.push(node.left);
            if (node.right != null) stack1.push(node.right);
        }
        //结果从反转栈中读取
        while (!stack2.isEmpty()) {
            res.add(stack2.pop());
        }
        return res;
    }
}
```

------

### 补充，前中后的递归解法

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        traverse(root,result);
        return result;
    }

    private void traverse(TreeNode node, List<Integer> result) {
        if(node == null){
            return;
        }
        traverse(node.left,result);
      	//添加位置不同
        result.add(node.val);
        traverse(node.right,result);
    }

}
```

------

### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

#### A.思路：序列化递归，终止条件为null，返回#,序列化左右，return 中，左，右；反序列化，分割字符串，数组转为队列，依次取第一个，调用自递归函数buildTree，终止#，返回null，创建当前结点，递归创建左右子数。

#### B.代码及注释：

```java
public class Codec {

    // Encodes a tree to a single string.
    //序列化前序遍历即可，碰到null设为#
    public String serialize(TreeNode root) {
        if(root == null) return "#,";
        //序列化左右子树
        String left = serialize(root.left);
        String right = serialize(root.right);
        //返回字符串为前序遍历
        return root.val+","+left+right;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] temp = data.split(",");
        //根据前序遍历构建树，需要每次拿出第一，如果为空则返回null，递归构建左右子树
        Queue<String> queue = new LinkedList<>(Arrays.asList(temp));
        return buildTree(queue);
    }

    private TreeNode buildTree(Queue<String> queue) {
        String s = queue.remove();
        if(s.equals("#")) return null;
        //构建顺序中左右
        TreeNode node = new TreeNode(Integer.parseInt(s));
        node.left = buildTree(queue);
        node.right = buildTree(queue);
        return node;
    }
}
```

------

### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

#### A.思路：外递归；TreeNode build(pre_root,in_left,in_right),参数需要前序（根节点坐标），中序（该子树的中序），节省空间，可以直接在总数组中传入坐标，即根为pre[pre_root],中序为in[in_left,in_right]

#### {终止：in_left>in_right return null；过程：构建root，找到总中序根的位置，来确定左右子树，递归构建左右子树

#### B.代码及注释：

```java
class Solution {
    //哈希表用来快速找inorder中根的位置
    HashMap<Integer,Integer> map = new HashMap<>();
    //扩大preorder作用域方便递归使用
    int[] preorder;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        //map存入inorder的值-坐标
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i],i);
        }
        //大构建
        return build(0,0,inorder.length-1);
    }

    //递归构建
    private TreeNode build(int pre_root,int in_left,int in_right){
        //终止条件，左子树边界>右
        if(in_left>in_right) return null;
        //当前根节点
        TreeNode root = new TreeNode(preorder[pre_root]);
        int in_root = map.get(root.val);
        //构建左子树,pre_root紧跟root（+1）,[in_left,in_right]在in_root左侧
        root.left = build(pre_root+1,in_left,in_root-1);
        //构建右子树,pre_root为根+左子树总数+1,[in_left,in_right]在in_root右侧
        root.right = build(pre_root+(in_root-in_left)+1,in_root+1,in_right);
        return root;
    }
}
```

------



### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

#### A.思路：；直接交换然后左右递归

#### B.代码及注释：

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        //递归终止条件
        if(root == null) return null;
        //对于每个节点，交换左右子树
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        //递归其左右节点
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

------



### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

#### A.思路：递归需要传入左右节点，然后递归其左节点的左节点，右节点的右节点；左节点的右节点，右节点的左节点

#### B.代码及注释：

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return dfs(root.left,root.right);
    }
    
    //递归left和right
    boolean dfs(TreeNode left,TreeNode right){
        //终止条件有至少有一边为空
        if(left == null && right == null) return  true;
        if(left == null || right == null)  return false;
        //如果不等
        if(left.val != right.val) return false;
        递归
        return dfs(left.left,right.right) && dfs(left.right,right.left);
    }
}
```

------



### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

#### A.思路：{终止：null｜｜p｜｜q return root；过程：递归求左右节点返回值，左右都不为null表示找到，返回root，有一边找到将其返回，否则返回空

#### B.代码及注释：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //递归终止条件，为空或者找到p，q。将该节点返回
        if(root == null|| root == p||root == q) return root;
        //递归找左右子节点的返回值
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        //如果两边都不为空则表示找到
        if(left!=null&&right!=null) {
            return root;
        }else if(left != null){
            //有一边找到，将其返回
            return left;
        }else if(right != null){
            return right;
        }
        //否则返回空
        return null;
    }
}
```

------

### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

#### A.思路：递归

#### B.代码及注释：

```java
class Solution {
    public int maxDepth(TreeNode root) {
        //终止条件return 0;
        if(root == null) return 0;
        //记录左右深度
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        //return 左右的最大值+1
        return Math.max(left,right)+1;
    }
}
```

------

### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

#### A.思路：递归

#### B.代码及注释：

```java
class Solution {
    int maxd= 0;
    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return maxd;
    }

    private int depth(TreeNode root) {
        if(root== null) return 0;
        int left = depth(root.left);
        int right = depth(root.right);
        maxd = Math.max(left+right,maxd);
        return Math.max(left,right)+1;
    }
}
```

------



### [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

#### A.思路：外递归；int maxHigh(root){终止：null return 0；过程：递归求左右节点maxHigh，返回最大值+1；剪枝：如果出现相差大于1，直接返回-1（小于0也返回-1）

#### B.代码及注释：

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        //平衡会返回高度，不平衡会返回-1
        return maxHigh(root)>=0;
    }

    //外递归求最大高度
    private int maxHigh(TreeNode node){
        //为空，高度为0
        if(node == null)
            return 0;
        int left = maxHigh(node.left);
        int right = maxHigh(node.right);
        //出现不平衡时，返回-1
        if(left < 0||right<0||Math.abs(left-right)>1){
            return -1;
        }else {
            //高度为左右最大值+1
            return Math.max(left,right) + 1;
        }
    }
}
```

------

### [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

#### A.思路：外递归；int maxHigh(root){终止：null return 0；过程：递归求左右节点返回值，每次更新res。返回最大值+当前值，如为负数则舍去返回0；

#### B.代码及注释：

```java
class Solution {
    int res = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return res;
    }

    private int dfs(TreeNode root) {
        //递归终止条件null返回0
        if(root == null) return 0;
        //递归判断左右的返回值
        int left = dfs(root.left);
        int right = dfs(root.right);
        //更新res
        res = Math.max(res,root.val+left+right);
        //返回值应为左右节点大的+当前值，如果小于0则舍弃下面的节点返回0
        int max = Math.max(left,right)+root.val;
        return max<0? 0:max;
    }
}
```

------



### [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

#### A.思路：void dfs(root，path){终止：左右孩子为空，res+当前节点return；过程：递归左右节点，记忆化加入path

#### B.代码及注释：

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        dfs(root,"");
        return res;
    }

    private void dfs(TreeNode node, String path) {
				//终止条件：左右为空，res加入val终止
        if(node.left == null && node.right == null){
            res.add(path + node.val);
            return;
        }
      	//深搜不为空的一边
        if(node.left!=null) {          
            dfs(node.left, path + node.val + "->");
        }
        if(node.right !=null) {
            dfs(node.right, path + node.val + "->");
        }
    }
}
```

------

### [129. 求根节点到叶节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

#### A.思路：void dfs(root，sum){终止：左右孩子为空，res+当前节点return；过程：递归左右节点，记忆化加入path

#### B.代码及注释：

```java
class Solution {
    int res = 0;
    public int sumNumbers(TreeNode root) {
        //dfs递归root，sum为0
        dfs(root, 0);
        return res;
    }
  
  
		//dfs递归root，传入sum
    public void dfs(TreeNode root, int sum) {

        //如果左右为空则需加入更新sum并加入res
        if(root.left == null&&root.right == null) {
            sum = sum*10+root.val;
            res += sum;
            return;
        }
        //如果左不为空，递归左
        if(root.left!=null) {
            dfs(root.left, sum*10+root.val);
        }
      	//如果右不为空，递归右
        if(root.right != null) {
            dfs(root.right, sum*10+root.val);
        }
    }
}
```



------



### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

#### A.思路：void dfs(s, left，right){终止：左右为0，res add当前sreturn；过程：左》右直接return。如果大于0，递归dfs（s+（，left-1，right）；

#### B.代码及注释：

```java
class Solution {
  	//初始化res
    List<String> res = new ArrayList<>();
  
    public List<String> generateParenthesis(int n) {	
        if(n==0) return res;
      	//dfs递归，初始为“”，n，n
        dfs("",n,n);
        return res;
    }
  
		//dfs递归传入path，以及剩余左右括号数
    private void dfs(String path, int left, int right) {
      	//终止条件：左右括号用完了，则加入res返回
        if(left==0&&right==0){
            res.add(s);
            return;
        }
      	//剪枝：剩余的左括号不能多于右括号
        if(left>right) return;
      	//加入左括号或者右括号
        if(left>0){
            dfs(s+"(",left-1,right);
        }
      	
        if(right>0){
            dfs(s+")",left,right-1);
        }
    }
}
```



------



### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

#### A.思路：boolean dfs(root，targetsum){终止：左右孩子为空，当前；过程：递归求左右节点返回值，有一个返回值为true则返回true

#### B.代码及注释：

```java
class Solution {
    
    public boolean hasPathSum(TreeNode root, int targetSum) {
        //树为空，返回false
        if (root == null) return false;
      	//dfs递归root，初始为targetSum
        return dfs(root, targetSum);
    }
		
  	//dfs递归root，传入剩下的targetSum
    private boolean dfs(TreeNode root, int targetSum) { 
    		//终止条件：左右为空，当前节点值为targerSum。
        if (root.left == null && root.right == null) {
            return targetSum == root.val;
        }
        //处理，减去节点值
        targetSum -= root.val;
        //递归左右子树
        if (root.left != null) {
            boolean left = hasPathSum(root.left, targetSum);
            //不能直接返回left，因为左右子树有一个为true即可返回true
            if (left) return true;
        }
      	
        if (root.right != null) {
            boolean right = hasPathSum(root.right, targetSum);
            if (right) return true;
        }
      	//左右都为false则返回false
        return false;
    }
}
```

------



# 四、排序算法

### [179. 最大数](https://leetcode-cn.com/problems/largest-number/)

#### A.思路：Arrays.sort(str,(a,b)->{return (b+a).compareTo(a+b);})

#### B.代码及注释

```java
class Solution {
    public String largestNumber(int[] nums) {
        String[] strs = new String[nums.length];
        //数组转为字符串
        for (int i = 0; i < nums.length; i++) {
            strs[i] = String.valueOf(nums[i]);
        }
      
        //字符串拼接排序，字典序
        Arrays.sort(strs,(a,b)->{
            return (b+a).compareTo(a+b);
        });
      
        //第一个为0则避免出现"000"
        if(strs[0].equals("0")){
            return "0";
        }
      
        //重新拼接res
        StringBuilder res = new StringBuilder();
        for (int i = 0; i < nums.length; i++) {
            res.append(strs[i]);
        }
        return res.toString();
    }
}
```

------

### [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

#### A.思路：从后往前找第一个比前数大的位i，i-1即为需要替换的数，[i,len-1]排序，找到第一个大于替换的数替换

#### B.代码及注释：

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        //从后往前找[len-1,1]，找到一个比前一个大的数，如12453找到5
        for (int i = len - 1; i > 0; i--) {
            if(nums[i] > nums[i-1]){
                //此时需要替换的数的位置为i-1
              	//对i之后的数进行排序
                Arrays.sort(nums,i,len);
              
                //从i开始往后找找到第一个比替换的数大的数
                for (int j = i; j < len; j++) {
                    if(nums[j] > nums[i-1]){
                      	//找到j进行交换
                        int temp  = nums[j];
                        nums[j] = nums[i-1];
                        nums[i-1] = temp;
                        return;
                    }
                }
            }
        }
        //如果没找到，表示最大了，只需要逆序，即排序即可
        Arrays.sort(nums);
        return;
    }
}
```

------

### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

#### A.思路：快速排序的基础

#### B.代码及注释：

```java
class Solution {
    public int[] exchange(int[] nums) {
      	//双指针对撞法
        int l = 0,r = nums.length-1;
        while (l<r){
          	//左边满足奇数++
            while (l<r && nums[l]%2 == 1) l++;
          	//右边满足偶数++
            while (l<r && nums[r]%2 == 0) r--;
          	//交换
            int temp = nums[l];
            nums[l] = nums[r];
            nums[r] = temp;
        }
        return nums;
    }
}
```

------



### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

#### A.思路：归并排序的基础

#### B.代码及注释：

```java
class Solution {
    //类似归并排序的两数组合并过程
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        //双指针指向两个数组末尾
        int l = m-1,r = n-1;
      
        //index指向开始填入数的开始
        int index = m+n-1;
      
        //都有数时比较大小，选择填入
        while (l>=0&&r>=0){
          	//填入较大的数
            if(nums1[l]>nums2[r]){
                nums1[index--] = nums1[l--];
            }else {
                nums1[index--] = nums2[r--];
            }
        }
        //只剩一个数组时全填入
        while (l>=0) nums1[index--] = nums1[l--];
        while (r>=0) nums1[index--] = nums2[r--];
    }
}
```

------

### [912. 排序数组(排序算法汇总)](https://leetcode-cn.com/problems/sort-an-array/)

#### A.思路：选择排序+快速排序+归并排序+堆排序

#### B.代码及注释：

#### **1.选择排序**

```java
private void selectSort(int[] nums){
    int len = nums.length;
    //双循环暴力
    for (int i = 0; i < len; i++) {
        //记录最小值坐标
        int min = i;
        //遍历i之后的数，找到最小坐标，交换
        for (int j = i+1; j < len; j++) {
            if(nums[j] < nums[min]) {
                min = j;
            }
        }
        swap(nums,i,min);	
    }
}

private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
}
```

#### **2.快速排序**

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        quickSort(nums, 0, len - 1);
        return nums;
    }
  
  	//quickSort [l,r]
    private void quickSort(int[] nums, int l, int r) {
          if(l<r) {
              //找到base值
              int pIndex = partition(nums, l, r);
              //递归排序左右两边
              quickSort(nums, l, pIndex - 1);
              quickSort(nums, pIndex + 1, r);
          }
    }

    //快速排序，返回base index，左边都比他小，右边都比他大
    private int partition(int[] nums, int l, int r ) {
        //随机选取base值
        int randomIndex = new Random().nextInt(r - l + 1) + l;
        swap(nums, l, randomIndex);
        // 基准值pivot选最前
        int pivot = nums[l];
        //双指针首位夹击
        int i = l,j = r;
        while (i<j){
            //j找一个比base小的数，i找一个比base大的数，交换
            while (i<j && nums[j]>= pivot) j--;
            while (i<j && nums[i]<= pivot) i++;
            swap(nums,i,j);
        }
        //双指针相遇时交换base
        swap(nums, l, i);
        return i;
    }
  	private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
		}
}
```

#### **3.归并排序**

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }

    private void mergeSort(int[] nums, int l, int r) {
        if (l < r) {
            int m = l + (r - l) / 2;
            //二分，分治
            mergeSort(nums, l, m);
            mergeSort(nums, m + 1, r);
            //归并
            merge(nums, l, m, r);
        }
    }
    
    //归并函数
    private void merge(int[] nums, int l, int m, int r) {
        //设置临时数组[r-l+1]存储归并排序值
        int[] temp = new int[r - l + 1];
        //i，j从两端出发
        int i = l, j = m + 1, index = 0;
        while (i <= m && j <= r) {
            if (nums[i] <= nums[j]) {
                temp[index++] = nums[i++];
            } else {
                temp[index++] = nums[j++];
            }
        }
        while (i <= m) temp[index++] = nums[i++];
        while (j <= r) temp[index++] = nums[j++];
        //临时数组结果存入数组
        for (int k = 0; k < temp.length; k++) {
            nums[k + l] = temp[k];
        }
    }
}
```



#### **4.堆排序**

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        //从最后一个非叶节点，siftDown操作维护了最大堆
        for (int i = (len - 1) / 2; i >= 0; i--) {
            siftDown(nums,i,len-1);
        }
        int i = len-1;
        //把堆顶即最大值换到最后，对堆顶进行范围siftDown
        while (i>0){
            swap(nums,i,0);
            i--;
            siftDown(nums,0,i);
        }
        return nums;
    }
  
    private void siftDown(int[] nums, int i, int end) {
        //定义最大值坐标max，以及左右孩子
        int max = i,left = 2*i+1,right = 2*i+2;
        //判断max是否为左右孩子
        if(left<=end && nums[left] > nums[max]){
            max = left;
        }
        if(right<=end&& nums[right]>nums[max]){
            max = right;
        }
        //如果是则对max与其交换，并对max进行siftDown操作
        if(max!=i){
            swap(nums,i,max);
            siftDown(nums,max,end);
        }
    }

    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
		}
}
```

------

### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

#### A.思路：倒序快速排序：当partition为k-1时返回；堆排序：维护一个k的最小堆，比堆顶大的元素添加一遍后，堆顶即为第k大

#### B.代码及注释：

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        //简化快排
        return findTopK(nums, 0, nums.length - 1, k);

    }

    //可以提前返回，修改quickSort即可
    private int findTopK(int[] nums, int l, int r, int k) {
        int i = partition(nums, l, r);
        if (i == k - 1) return nums[i];
        else if (i > k - 1) return findTopK(nums, l, i - 1, k);
        else {
            return findTopK(nums, i + 1, r, k);
        }
    }

    private int partition(int[] nums, int l, int r) {
        //快排返回base的坐标
        int randomIndex = l + new Random().nextInt(r - l + 1);
        swap(nums, l, randomIndex);
        int pivot = nums[l];
        int i = l, j = r;
        while (i < j) {
            while (i < j && nums[j] <= pivot) j--;
            while (i < j && nums[i] >= pivot) i++;
            swap(nums, i, j);
        }
        swap(nums, i, l);
        return i;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        //k数组
        int[] res = new int[k];
        int i = 0;
        for (; i < k; i++) {
            res[i] = nums[i];
        }
        //维护数组为最小堆
        buildHeap(res);
        //比堆顶大的剩下元素添加进去，维护堆
        while (i < nums.length) {
            if (nums[i] > res[0]) {
                res[0] = nums[i];
                siftDown(res, 0,res.length-1);
            }
            i++;
        }
        return res[0];
    }

    //从从最后一个非叶子节点往
    private void buildHeap(int[] arr) {
        int len = arr.length;
        for (int i = (len - 1) / 2; i >= 0; i--)
            siftDown(arr, i,len-1);
    }

    //调整使得i在范围中最小，siftDown操作
    private void siftDown(int[] arr, int i,int end) {
        int min = i;
        int left = 2*i+1,right = 2*i+2;
        if (left <= end && arr[left] < arr[min])
            min = left;
        if (right <= end && arr[right] < arr[min])
            min = right;
        if (min != i) {
            swap(arr, min, i);
            //parent变动后，其子树也需要一直
            siftDown(arr, min,end);
        }
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

#### C.补充：调用API接口

```java
class Solution {
    //API解法
    public int findKthLargest(int[] nums, int k) {
        Queue<Integer> pq = new PriorityQueue<>((v1,v2)->v1-v2);
        int i =0;
        for (; i < k; i++) {
            pq.add(nums[i]);
        }
        while (i<nums.length){
            if(nums[i]>pq.peek()){
                pq.remove();
                pq.add(nums[i]);
            }
            i++;
        }
        return pq.peek();
    }
}
```

------

### [253. 会议室 II](https://leetcode-cn.com/problems/meeting-rooms-ii/)

#### A.思路：最小堆维护会议结束时间，最前面的表示最早结束的。陆续比较peek，poll然后add

#### B.代码及注释：



```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        //优先队列维护最小堆，即最早结束的时间
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        //按照开始时间排序
        Arrays.sort(intervals,(a,b)->a[0]-b[0]);
        //把第一个加进去
        queue.add(intervals[0][1]);
        //依次按开始时间，如果开始时间大于等于优先队列的最早结束时间，可以poll再add
        for (int i = 1; i < intervals.length; i++) {
            int last = queue.peek();
            if(last<=intervals[i][0]){
                queue.poll();
            }
            queue.add(intervals[i][1]);
        }
        return queue.size();
    }
}
```

------



### [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

#### A.思路：归并排序中，在merge时出现逆序对可以判断

#### B.代码及注释：

```java
class Solution {
    int count;
    //归并排序模式基础加入一个count表示结果
    public int reversePairs(int[] nums) {
        this.count = 0;
        merge(nums,0,nums.length-1);
        return count;
    }

    private void merge(int[] nums, int l, int r) {
        int m = l+(r-l)/2;
        if(l<r){
            merge(nums,l,m);
            merge(nums,m+1,r);
            mergeSort(nums,l,m,r);
        }
    }

    private void mergeSort(int[] nums, int l, int m, int r) {
        int[] temp = new int[r-l+1];
        int index = 0;
        int i = l,j = m+1;
        while (i<=m && j<=r){
            if(nums[i] <= nums[j]){
                temp[index++] = nums[i++];
            }else {
                //当j数组大于i数组出现逆序对，数量为i数组剩余数的个数
                count += (m-i+1);
                temp[index++] = nums[j++];
            }
        }
        while (i<=m) temp[index++] = nums[i++];
        while (j<=r) temp[index++] = nums[j++];
        for (int k = 0; k < temp.length; k++) {
            nums[k+l] = temp[k];
        }
    }
}
```

------

### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

#### A.思路：找到链表中点，断开归并法排序

#### B.代码及注释：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        //左中点断开
        ListNode m = midNode(head);
        ListNode r = m.next;
        m.next = null;
        //递归排序左右链表
        ListNode left = sortList(head);
        ListNode right = sortList(r);
        //合并两链表
        return mergeTwoLists(left,right);
    }
    //合并两链表
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy  = new ListNode(-1);
        ListNode cur = dummy;
        while (l1 != null && l2!= null){
            if(l1.val<=l2.val){
                cur.next = l1;
                l1 = l1.next;
            }else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 == null? l2:l1;
        return dummy.next;
    }
    //找到链表左中点
    private ListNode midNode(ListNode head) {
        if(head == null||head.next == null) return head;
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next!=null&&fast.next.next!=null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```















### [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

#### A.思路：按照下界排序，设定start、end开始判断合并，加入res数组

#### B.代码及注释：

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0) return new int[][]{};
        //按照下届排序
        Arrays.sort(intervals,(a,b)->(a[0] - b[0]));
        //res数组
        int[][] res = new int[intervals.length][2];
        //index用来指向res
        int index = 0;
        //start，end用来合并，并加入res
        int start = intervals[0][0],end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            //如果end大于等于该数下届合并
            if(intervals[i][0]<=end){
                end = Math.max(intervals[i][1],end);
                //不合并则加入res，并重新初始化start，end
            }else {
                res[index][0] = start;
                res[index][1] = end;
                index++;

                start = intervals[i][0];
                end   = intervals[i][1];
            }
        }
        //最后还要加一次
        res[index][0] = start;
        res[index][1] = end;
        return Arrays.copyOfRange(res,0,index+1);
    }
}
```

------



# 五、双指针

### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

#### A.思路：排序+双指针+去重复

#### B.代码及注释：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        //初始化结果集
        List<List<Integer>> res = new ArrayList<>();
        int len = nums.length;
        if(len<3) return res;
        //对数组进行排序
        Arrays.sort(nums);
        //遍历数组，另外在找其右边的两数
        for (int i = 0; i < len; i++) {
            //如果当前数大于0，则三数和一定大于0，结束
            if(nums[i] >0) break;
            //与上个数相等不用重复判断，去重，保证nums[i]的唯一性
            if(i>0 && nums[i] == nums[i-1]) continue;
            //定义l与r
            int l = i+1,r = len-1;
            //控制双指针缩小范围
            while (l<r){
                int sum = nums[i] + nums[l] + nums[r];
                //如果和为0，表示找到结果
                if(sum == 0){
                    //Arrays.asList生产结果path，加入到res中
                    res.add(Arrays.asList(nums[i],nums[l],nums[r]));
                    //左右指针去重
                    while (l<r && nums[l] == nums[l+1]) l++;
                    while (l<r && nums[r] == nums[r-1]) r--;
                    l++;
                    r--;
                }
                else if(sum>0) r--;
                else if(sum<0) l++;
            }
        }
        return res;
    }
}
```

------

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

#### A.思路：一个指针找非零数，一个指针在前面指向应该填充的位置

#### B.代码及注释：

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums == null) return;
        //index表示应该填充位置
        int index = 0;
        for (int i = 0; i < nums.length; i++) {
            //找到非0交换
            if(nums[i]!=0){
                int temp = nums[i];
                nums[i] = nums[index];
                nums[index]=temp;
                index++;
            }
        }
    }
}
```

------

### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

#### A.思路：对撞指针，每次移动短板计算res

#### B.代码及注释：

```java
class Solution {
    public int maxArea(int[] height) {
        int len = height.length;
        int l = 0 ,r =len-1;
        int res = 0;
        while (l<r){
            if(height[l]<height[r]){
                res = Math.max(res,(r-l)*height[l++]);
            }else {
                res = Math.max(res,(r-l)*height[r--]);
            }
        }
        return res;
    }
}
```

------



### [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

#### A.思路：对撞指针，非字母数字跳过，不相等跳出循环。

#### B.代码及注释：

```java
class Solution {
    public boolean isPalindrome(String s) {
        int  l = 0,r = s.length()-1;
        String s1 = s.toLowerCase();
        boolean flag =true;
        while (l<r){
            char a = s1.charAt(l),b = s1.charAt(r);
            if(!((a>='0'&&a<='9')||(a>='a'&&a<='z'))){
                l++;
                continue;
            }
            if(!((b>='0'&&b<='9')||(b>='a'&&b<='z'))){
                r--;
                continue;
            }
            if(a!=b){
                flag = false;
                break;
            }
            l++;
            r--;
        }
        return flag;
    }
}
```

------

### [214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

#### A.思路：对撞指针，每次是在前面加，相当于反转字符串，从index=len-1到1开始判断是否需要加入，如果[0,index]为回文穿则不需要加入

#### B.代码及注释：

```java
class Solution {
    public String shortestPalindrome(String s) {
        char[] chars = s.toCharArray();
        //相当于从翻转字符串第一位开始判断是否加入
        int index = s.length()-1;
        //初始化一个需要加入在s前面的sb
        StringBuilder sb = new StringBuilder();
        while (index>=0){
            //默认不需要加入
            boolean flag = false;
            //判断[0,index]是否为回文
            for(int i=0,j=index;i<=j;i++,j--){
                if(chars[i] != chars[j]){
                    //需要加入该index
                    flag = true;
                    break;
                }
            }
            if(flag){
                sb.append(chars[index]);
            }else {
                break;
            }
            index--;
        }
        return sb.append(s).toString();
    }
}
```

------

### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

#### A.思路：设立l，r，t，b四个边界条件，四个方向分布循环遍历，判断结束用break跳出循环

#### B.代码及注释：

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        //初始化结果集
        List<Integer> res = new ArrayList<>();
        //确定l，r，t，b边界条件
        int l =0,r=matrix[0].length-1;
        int t=0,b=matrix.length-1;
        //while（true）一直循环，用break跳出
        while (true){
            //左-》右，j=[l,r] i=t，循环结束t++，直到大于b
            for (int i = l; i <= r; i++) {
                res.add(matrix[t][i]);
            }
            if(++t >b ) break;
            //上-》下，i=[t,b] j=r，循环结束r--，直到小于l
            for (int i = t; i <= b; i++) {
                res.add(matrix[i][r]);
            }
            if(--r <l ) break;
            //右-》左，j=[r,l] i=b，循环结束b--，直到小于t
            for (int i = r; i >= l; i--) {
                res.add(matrix[b][i]);
            }
            if(--b < t ) break;
            //下-》上，i=[b,t] i=l，循环结束l++，直到大于r
            for (int i = b; i >= t; i--) {
                res.add(matrix[i][l]);
            }
            if(++l >r ) break;
        }
        return res;
    }
}
```

------

### [59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

#### A.思路：设立l，r，t，b四个边界条件，四个方向分布循环遍历，判断结束可以用数量n^2；

#### B.代码及注释：

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int l =0,r = n-1,t = 0,b = n-1;
        int[][] res = new int[n][n];
        int start = 1,end = n*n;
        //正方形，必定会走四个循环，用数量判断终止
        while (start<=end){
            for (int i = l; i <= r; i++) {
                res[t][i] = start++;
            }
            t++;
            for (int i = t; i <= b; i++) {
                res[i][r] = start++;
            }
            r--;
            for (int i = r; i >= l; i--) {
                res[b][i] = start++;
            }
            b--;
            for (int i = b;i>=t;i--){
                res[i][l] = start++;
            }
            l++;
        }
        return res;
    }
}
```



------

### [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

#### A.思路：matrix\[i][j] = matrix\[len-1-j][len-1-i];表示对角线折叠

#### B.代码及注释：

```java
class Solution {
    public void rotate(int[][] matrix) {
        int len = matrix.length;
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len - i; j++) {
                int temp = matrix[i][j];
                //正对角线折叠
                matrix[i][j] = matrix[len-1-j][len-1-i];
                matrix[len-1-j][len-1-i] = temp;
            }
        }

        for (int i = 0; i < len / 2; i++) {
            for (int j = 0; j < len; j++) {
                int temp = matrix[i][j];
                //行折叠
                matrix[i][j] = matrix[len-1-i][j];
                matrix[len-1-i][j]=temp;
            }
        }
    }
}
```



# 六、哈希表优化

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

#### A.思路：哈希查找空间O(N)实现时间O(N)

#### B.代码及注释：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        /*暴力双循环
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i+1; j < n; j++) {
                if(nums[i] + nums[j] == target){
                    return new int[]{i,j};
                }
            }
        }
        return new int[0];*/
      
      	//哈希表法
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            //找到target-nums[i]
            if(map.containsKey(target - nums[i])){
                return new int[]{i,map.get(target - nums[i])};
            }
            //map存储数值-下标
            map.put(nums[i],i);
        }
        return new int[0];
    }
}
```

------

### [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

#### A.思路:哈希表HashMap+双向链表LinkedList，O(1)

#### B.代码及注释：

```java
class LRUCache {
    //数据存储格式Node(key,val)
    private class Node{
        int key,val;

        private Node(int k,int v){
            this.key = k;
            this.val = v;
        }
    }
    
    //哈希表
    private Map<Integer,Node> map;
    //双向链表
    private LinkedList<Node> list;
    //初始容量
    private int capacity;

    //构造函数直接初始化成员变量
    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>(capacity);
        list = new LinkedList<>();
    }
    //get方法，使用完需要更新，可以和put方法合并
    public int get(int key) {
        if(!map.containsKey(key)){
            return -1;
        }
        int val = map.get(key).val;
        //更新相当于put一个新的
        put(key,val);
        return val;
    }
    
    //put方法
    public void put(int key, int value) {
        Node n = new Node(key,value);
        //如果已存在key，list先删后加头，map需要put
        if(map.containsKey(key)){
            list.remove(map.get(key));
            list.addFirst(n);
            map.put(key,n);
        }else {
            //不存在，如果capacity已满，list需要删除尾部，map删除key
            if(list.size() == capacity){
                Node last = list.removeLast();
                map.remove(last.key);
            }
            //list加在头，map需要put
            list.addFirst(n);
            map.put(key,n);
        }
    }
```

#### C.补充：自己实现双向链表DoubleList

```java
private class DoubleList{
    //自定义首位相连的节点和size
    Node dummy = new Node(0,0);
    Node tail = new Node(0,0);
    int size;
    
    //构造函数首尾相连，size=0
    private DoubleList(){
        dummy.next = tail;
        tail.prev = dummy;
        size = 0;
    }
    
    //加头
    private void addFirst(Node n){
        Node next = dummy.next;
        dummy.next = n;
        next.prev = n;
        n.next = next;
        n.prev = dummy;
        size++;
    }
    
    //remove:prev的next指向next;next的prev指向prev
    private void remove(Node n){
        n.prev.next = n.next;
        n.next.prev = n.prev;
        n.next = null;
        n.prev = null;
        size--;
    }
    
    //remove(tail.prev)
    private Node removeLast(){
        Node last = tail.prev;
        remove(last);
        return last;
    }

    private int getSize(){
        return size;
    }

}
```

------

### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

#### A.思路:哈希set优化，先找前置，然后往后遍历记录最长

#### B.代码及注释：

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        int res = 0;
        for (int num : nums) {
            //利用HashSet存储，先判断是否有前面的。
            if(!set.contains(num-1)){
                int cur =num;
                int l = 1;
                //开始往后遍历找寻
                while (set.contains(num+1)){
                    num++;
                    l++;
                }
                res = Math.max(l,res);
            }
        }
        return res;

    }
}
```

------



### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

#### A.思路:哈希表优化，位运算优化，异或^(^0为自身，^自身为0)

#### B.代码及注释：

```java
class Solution {
    public int singleNumber(int[] nums) {
       /*
       //哈希表记录nums-出现次数
       Map<Integer,Integer> map = new HashMap<>();
       for (int num :nums){
           Integer value = map.get(num);
           value = value == null? 1:++value;
           map.put(num,value);
       }
       for (Integer num : map.keySet()) {
           Integer value = map.get(num);
           if(value==1){
               return num;
           }
        }
        return -1;*/
        //异或算法
        int res =nums[0];
        for (int i = 1; i < nums.length; i++) {
            res =res^nums[i];
        }
        return res;
    }
}
```



------

### [41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

#### A.思路:遍历nums[i],nums[i]应该放在mums[nums[i]-1]的位置，然后在遍历判断num[i]和i+1

#### B.代码及注释：

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        //nums的i位置应该放在其值-1的位置即nums[i] = nums[nums[i]-1]
        for (int i = 0; i < len; i++) {
            //只放在范围内的正数
            while (nums[i] > 0 && nums[i] <= len && nums[i] != nums[nums[i] - 1]){
                swap(nums,nums[i] -1,i);
            }
        }
        //不满足i位置放i+1的直接跳出
        for (int i = 0; i < len; i++) {
            if(nums[i] != i+1){
                return i+1;
            }
        }
        //否则返回len+1
        return len+1;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

------

### [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

#### A.思路:hashmap存储节点和新节点

#### B.代码及注释：

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null) return null;

        //创建哈希表
        Map<Node,Node> map = new HashMap<>();

        Node cur = head;
        //map存储节点和新节点
        while (cur!=null){
            Node newNode = new Node(cur.val);
            map.put(cur,newNode);
            cur = cur.next;
        }
        cur = head;
        //用map设置next和random
        while (cur!=null){
            Node newNode = map.get(cur);
            if(cur.next != null){
                newNode.next = map.get(cur.next);
            }
            if(cur.random != null){
                newNode.random = map.get(cur.random);
            }
            cur =cur.next;
        }
        return map.get(head);
    }
}
```

------



### [523. 连续的子数组和](https://leetcode-cn.com/problems/continuous-subarray-sum/)

#### A.思路:前缀和+hashset存储余数，余数相同一定相减为倍数

#### B.代码及注释：

```java
class Solution {
/*    public boolean checkSubarraySum(int[] nums, int k) {
        int len = nums.length;
        //sum[j]表示[0,j-1]的和
        int[] sum = new int[len+1];

        for (int i = 1; i <= len; i++) {
            sum[i] = sum[i-1] + nums[i-1];
        }

        for (int i = 0; i < len; i++) {
            for (int j = i+2; j <= len; j++) {
                if((sum[j] - sum[i])%k == 0) return true;
            }
        }
        return false;
    }*/

    public boolean checkSubarraySum(int[] nums, int k) {
        int len = nums.length;
        //sum[j]表示[0,j-1]的和
        int[] sum = new int[len+1];

        for (int i = 1; i <= len; i++) {
            sum[i] = sum[i-1] + nums[i-1];
        }
        Set<Integer> set = new HashSet<>();
        for (int i = 2; i <= len; i++) {
            set.add((sum[i-2]%k));
            if(set.contains(sum[i]%k)) return true;
        }
        return false;
    }
}
```

------



# 七、二分查找

### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

#### A.思路：有序数组，缩紧边界

#### B.代码及注释：

```java
class Solution {
    public int search(int[] nums, int target) {
        //初始化[l,r]区间
        int l = 0,r = nums.length-1;
        //终止时l>r
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (nums[m] == target) {
                return m;
            } else if (nums[m] < target) {
                //缩紧左边界
                l = m +1;
            } else {
                //缩紧右边界
                r = m -1;
            }
        }
        return -1;
    }
}
```

------

### [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

#### A.思路：摊开，在用二分搜索，m = mid/col；n = mid%col

#### B.代码及注释：

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length;
        int col = matrix[0].length;
        int l = 0,r = row*col-1;
        while (l<=r){
            int mid = l + (r-l)/2;
            int rownum = mid/col;
            int colnum = mid%col;
            if(matrix[rownum][colnum] == target){
                return true;
            }else if(matrix[rownum][colnum] > target){
                r = mid -1;
            }else {
                l = mid +1;
            }

        }
        return false;
    }
}
```



### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

#### A.思路：从左下角向右、上搜

#### B.代码及注释：

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int i = m-1,j=0;
        while (i>=0&&j<n){
            if(matrix[i][j] == target){
                return true;
            }else if(matrix[i][j]>target){
                i--;
            }else {
                j++;
            }
        }
        return false;
    }
}
```

------



### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

#### A.思路：有序数组，额外加一层if判断在那边

#### B.代码及注释：

```java
class Solution {
    public int search(int[] nums, int target) {
        int len = nums.length;
        if(len == 0) return -1;
        int l = 0, r = len-1;
        while (l<=r){
            int m = l+(r-l)/2;
            if(nums[m] == target) return m;
            //通过比较m处值和最右值，判断哪边有序
            if(nums[m]<nums[r]){
                if(nums[m] <target && target<=nums[r])
                    l=m+1;
                else r=m-1;
            }else {
                if(nums[m]>target && target>= nums[l])
                    r = m-1;
                else l = m+1;
            }
        }
        return -1;
    }
}
```

------



### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

#### A.思路：有序数组，判断i与nums[i]即可

#### B.代码及注释：

```java
class Solution {
    public int missingNumber(int[] nums) {
       int l = 0,r = nums.length-1;
       while (l<=r){
           int m = (l+r)/2;
           if(nums[m] == m) l=m+1;
           if(nums[m] != m) r=m-1;
       }
       return l;
    }
}
```

















### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

#### A.思路：找左边界（返回l，r在小于target的位置可能左越界），找右边界（返回r，l在大于target的位置可能右越界）

#### B.代码及注释：

```java
class Solution {
    //使用的二分模版，结束时l>r;
    public int[] searchRange(int[] nums, int target) {
        int l = left_bound(nums,target);
        int r = right_bound(nums,target);
        //不存在时，l>r
        if(l > r) return new int[]{-1,-1};
        return new int[]{l,r};
    }
    //找左边界,返回l，r一定在比target小的位置（可能左越界）
    private  int left_bound(int[] nums, int target) {
        int l = 0 ,r = nums.length-1;
        while (l<=r){
            int m = l+(r-l)/2;
            //相等时继续缩紧右边界,跳出时，r跑到l左边
            if(nums[m] == target){
                r=m-1;
            }else if(nums[m]<target){
                l = m+1;
            }else {
                r = m-1;
            }
        }
        return l;
    }
    //找右边界,返回r，l一定在比target大的位置（可能右越界）
    private int right_bound(int[] nums, int target) {
        int l = 0 ,r = nums.length-1;
        while (l<=r){
            int m = l+(r-l)/2;
            //相等时继续缩紧左边界,跳出时，l跑到r右边
            if(nums[m] == target){
                l=r+1;
            }else if(nums[m]<target){
                l = m+1;
            }else {
                r = m-1;
            }
        }
        return r;
    }
}
```

------

### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

#### A.思路：二分法找

#### B.代码及注释：

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int l = 0,r = nums.length-1;
        //二分法找
        while (l<r){
            int m = l + (r-l)/2;
            //m处于一个递减序列
            if(nums[m] > nums[m+1]){
                r=m;
            }else {
                l = m+1;
            }
        }
        return r;
    }
}
```

------

### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

#### A.思路：二分与牛顿迭代法

#### B.代码及注释：

```java
class Solution {
    public int mySqrt(int x) {
        if(x==1||x==0) return x;
        int l = 0,r =x;
        while (l<=r){
            int m = l+(r-l)/2;
            if(x/m < m){
                r =m-1;
            }else if(x/m>m){
                l = m+1;
            }else
                return m;
        }
        return r;
    }
    //牛顿迭代法
    public int mySqrt2(int x){
        if(x==0 ) return 0;
        double a = x , x0 = x;
        while (true){
            double xi = 0.5*(x0 +a/ x0);
            if(Math.abs(x0-xi)<1e-7){
                break;
            }
            x0 = xi;
        }
        return (int) x0;
    }
}
```

------



# 八、动态规划（最值问题）

### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

#### A.思路：dp始祖，dp[i] = dp[i-1]+dp[i-2]

#### B.代码及注释：

```java
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1]+dp[i-2];
            dp[i]%=1000000007;
        }
        return dp[n];
    }
}
```

------

### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

#### A.思路：dp始祖演变，由于一次只能爬一层或两层，相当于可以从n-1或者n-2上来，dp[i] = dp[i-1]+dp[i-2]

#### B.代码及注释：

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <=n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
```

------

### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

#### A.思路：dp始祖演变，由于不能连着偷，只能偷dp[i-2]+nums[i];

#### B.代码及注释：

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = nums[0];
        for (int i = 2; i <= n; i++) {
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i-1]);
        }
        return dp[n];
    }
}
```

------

### 补充题：0-9构成一个环，可以顺时针或逆时针走一步，请问走n步回到0步共有多少走法？

#### A.思路：dp始祖演变，dp[i\][j] 表示走i步到j点的走法数；递推公式：走n步到0的方案=走n-1步到1的方案+走n-1步到9的方案。dp[i][j\] = dp[i-1\][(j-1+length)%length\]+dp[i-1\][(j+1)%length\]

#### B.代码及注释：





------



### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

#### A.思路：最长子序！不连续：与前面的每一个都有关，初始化为dp[i]=1,dp[i]以nums[i]结尾的最长子序，当num[i]>前面某个数，则判断dp[i] = max（dp[i],dp[j]+1)

#### B.代码及注释：

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int res = 1;
        //dp表示以dp[i]结尾。初始化为1
        int[] dp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            dp[i] = 1;
        }
        //填表，双循环，与i前面的每一个数做比较
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                //如果该数产生递增，更新dp[i]
                if(nums[j]<nums[i])
                    dp[i] = Math.max(dp[i],dp[j] +1);
            }
            res = Math.max(dp[i],res);
        }
        return res;
    }
}
```



### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

#### A.思路：最大！连续：与前一个有关，初始化为nums[0],dp[i]以nums[i]结尾的最大和，直接在nums[]上更新，初始res=nums[0],if(nums[i-1]>0)则nums[i]加上，否则不变。

#### B.代码及注释：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //初始dp和res
        int res = nums[0]；
        for (int i = 1; i < nums.length; i++) {
            //递推共识
            if(nums[i-1] > 0) nums[i] += nums[i-1];
            //不断更新res
            res = Math.max(res,nums[i]);
        }
        return res;
    }
}
```

#### C.变种：返回索引，设置start，end记录每次的，设置finalStart，finalEnd记录最终的

```java
class Solution {
    public int[] maxSubArray(int[] nums) {
        //初始dp和res
        int res = nums[0];
        //变种
        int start=0,end=0;
        int finalStart=0,finalEnd=0;
        for (int i = 1; i < nums.length; i++) {
            //递推公示
            if(nums[i-1] > 0) {
                nums[i] += nums[i-1];
                //更新结尾
                end = i;
            }else {
                start=i;
                end=i;
            }
            //更新时更新final
            if(nums[i]>res) {
                res = nums[i];
                finalEnd = end;
                finalStart=start;
            }
        }
        return new int[]{finalStart,finalEnd};
    }
}
```

------

### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)dp！！！dp！！！！dp！！！！dp！！！

#### A.思路：最长有效括号，需要判断()和（））两种情况，注意判断不能超过界限，否则返回0

#### B.代码及注释：

```java
class Solution {
    public int longestValidParentheses(String s) {
        int res = 0;
      
        //dp表示以s[i]结尾的最长有效括号
        int[] dp = new int[s.length()];
        for (int i = 1; i < s.length(); i++) {
          
            //当）开始记录，并且每次判断注意不能超界
            if (s.charAt(i) == ')') {
              
                //情况1：前一个组合（）为dp【i-2】+2
                if (s.charAt(i - 1) == '(') {
                    dp[i] = (i - 2 >= 0 ? dp[i - 2] : 0) + 2;
                  
                  
                    //情况2：前一个不组合（）（））需要找到i-dp[i-1]-1位置判断是否组合：为（
                    //形成组合则为dp[i-1]+2+dp[i-dp[i-1]-2]
                } else if ((i - dp[i - 1] - 1) >= 0 && s.charAt(i - dp[i - 1] - 1) == '(') {
                    dp[i] = dp[i - 1] + 2 + ((i - dp[i - 1] - 2) >= 0 ? dp[i - dp[i - 1] - 2] : 0);
                }
            }
          
```

------

### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

#### A.思路：最少硬币，dp[i]表示i金额的最少硬币，初始为i+1即可。每次遍历硬币，找到之前对应的金额+1

#### B.代码及注释：

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        //初始化令填最大的amount+1,0为0
        Arrays.fill(dp,amount+1);
        dp[0] = 0;
        //填表
        for (int i = 1; i <= amount; i++) {
            //遍历硬币
            for (int coin : coins) {
                //当总金额大于硬币时，dp开始依次换硬币更新为最小值
                if(i >= coin){
                    dp[i] = Math.min(dp[i],dp[i-coin] + 1);
                }
            }
        }
        return dp[amount]== amount+1? -1:dp[amount];
    }
}
```

------

### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

#### A.思路：硬币组合，类似于爬楼梯问题。组合问题先遍历物品，dp[i] += dp[i-coin]

#### B.代码及注释：

```java
class Solution {
    public int change(int amount, int[] coins) {
        //相当于另一种跳楼梯问题
        int[] dp = new int[amount+1];
        //初始化为0,do[0]初始化为1，只要能减到0，表示有一种
        Arrays.fill(dp,0);
        dp[0] = 1;
        //求组合（先遍历物品），求排列（直接填表，每次都遍历物品）
        for (int coin : coins) {
            for (int i = coin; i < amount + 1; i++) {
                dp[i] += dp[i-coin];
            }
        }
        return dp[amount];
    }
}
```

------



### [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

#### A.思路：最大正方形！初始化为0，增加一行一列，所以dp[m+1\][n+1\],当当前为1时，dp\[i][j]为左，左上、上最小值+1。表示最大边长

#### B.代码及注释：

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int res = 0;
        //初始化为0，每个数都需要判断额外增加一行一列
        int[][] dp = new int[m+1][n+1];
        //填表
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                //当前为1时，取左、左上、上最小值+1（res表示边长）
                if(matrix[i-1][j-1] == '1'){
                    dp[i][j] = 1+Math.min(dp[i-1][j-1],Math.min(dp[i-1][j],dp[i][j-1]));
                    res = Math.max(res,dp[i][j]);
                }
            }
        }
        return res*res;
    }
}
```

------

### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

#### A.思路：最大路径！初始点为0，第一行第一列只能向右向下，初始化。所以dp\[i][j]为左，上相加。

#### B.代码及注释：

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        //左上都为1
        for (int i = 0; i < n; i++) {
            dp[0][i] = 1;
        }
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        //为左、上之和
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```

------

### [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

#### A.思路：最大路径！有障碍物的时候必须判断是否为0在更新

#### B.代码及注释：

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[][] dp = new int[m][n];
        //左上都为1,如果为障碍物就为0
        for (int i = 0; i < n && obstacleGrid[0][i] == 0; i++) {
            dp[0][i] = 1;
        }
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) {
            dp[i][0] = 1;
        }
        //为左、上之和
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if(obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

------







### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

#### A.思路：最小路径和！初始点为0，第一行第一列只能向右向下，初始化。所以dp\[i][j]为左，上最小值+grid[i][j\]。

#### B.代码及注释：

```java
class Solution {
    public int minPathSum(int[][] grid) {
        for (int i = 0; i < grid.length;i++) {
            for (int j = 0; j < grid[0].length;j++) {
                //初始化
                if(i==0&&j==0) continue;
                //初始化第一列第一行
                else if(i==0) grid[0][j] = grid[0][j-1] + grid[0][j];
                else if(j==0) grid[i][0] = grid[i-1][0] + grid[i][0];
                else {
                    //左上最小值+当前值
                    grid[i][j] = Math.min(grid[i-1][j],grid[i][j-1]) + grid[i][j];
                }
            }
        }
        return grid[grid.length-1][grid[0].length-1];
    }
}
```

------



### [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

#### A.思路：最长子序列！初始化为空字符串，所以dp[m+1\][n+1\],dp\[i][j]表示前i数组和前j数组的公共部分。当前数字相等，则为dp[i-1][j-1\]+1，更新res这里要求连续与下一题子序列不同的是

#### B.代码及注释：

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[][] dp = new int[m+1][n+1];
        int res = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                //与子序列不同的是，这里要求连续，只有相等时才更新
                if(nums1[i-1] == nums2[j-1]){
                    dp[i][j] = dp[i-1][j-1]+1;
                    res = Math.max(res,dp[i][j]);
                }
            }
        }
        return res;
    }

```

------



### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

#### A.思路：最长子序列！初始化为空字符串，所以dp[m+1\][n+1\],dp\[i][j]表示前i字符和前j字符的公共部分。当前字符相等，则为dp[i-1][j-1\]+1;否则取左、上最大值

#### B.代码及注释：

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        //初始化为空字符串，dp[i][j]表示前i个，前j个
        int[][] dp = new int[n1+1][n2+1];
        //填表
        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                //当此时字母相等时，为之前加一
                if(text1.charAt(i-1) == text2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else {
                    //不相等取前后最大值
                    dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[n1][n2];
    }
}
```

------









### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

#### A.思路：最长子串！初始化为dp[i\][i\]=true,dp\[i][j]表示[i,j]是否为回文子串,chars[i]==chars[j],dp\[i][j]==dp\[i+1][j-1]

#### B.代码及注释：//打印一下呀

```java
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if(len < 2) return s;
        //res记录最长值，begin记录起始index
        int res=1;
        int start=0;
        //dp[i][j]表示[i,j]是否为回文子串
        boolean[][] dp = new boolean[len][len];
        //初始化，自身为回文
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }
        //字符串转化为数组方便读取
        char[] chars = s.toCharArray();
        //填表，i[0,j),j[1,len)
        for (int j = 1; j < len; j++) {
            for (int i = 0; i < j; i++) {
                //不等时肯定为false
                if(chars[i] != chars[j]) dp[i][j] =false;
                else {
                    //首尾相等则判断中间段,j-i大于1才有中间值
                    if(j-i<2) {
                        dp[i][j]=true;
                    } else {
                        dp[i][j] = dp[i+1][j-1];
                    }
                }
                //更新完判断是否为最大，更新res和start
                if(dp[i][j] && j-i+1>res){
                    res = j-i+1;
                    start=i;
                }
            }
        }
        return s.substring(start,start+res);
    }
}
```

### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

#### A.思路：最少步骤！初始化为空字符串，所以dp[m+1\][n+1\],dp\[i][j]表示前i字符转为前j字符的最小步骤

#### B.代码及注释：

```java
class Solution {
    public int minDistance(String word1, String word2) {
        //初始化dp[n1+1][n2+1]
        int n1 = word1.length();
        int n2 = word2.length();
        int[][] dp = new int[n1+1][n2+1];
        //由空，转为字符串，一定是插入步骤依次+1
        for (int j = 1; j <= n2; j++) {
            dp[0][j] = dp[0][j-1] + 1;
        }
        //由字符串转为空，一定是删除步骤
        for (int i = 1; i <= n1; i++) {
            dp[i][0] = dp[i-1][0] + 1;
        }
        //填表
        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                //当前位置字符相同，不用操作：dp[i][j] = dp[i-1][j-1]
                if(word1.charAt(i-1) == word2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    //dp[i-1][j-1]为替换操作。dp[i-1][j]为删除操作。dp[i][j-1]为插入操作
                    dp[i][j] = Math.min(Math.min(dp[i-1][j-1],dp[i-1][j]),dp[i][j-1])+1;
                }
            }
        }
        return dp[n1][n2];
    }
}
```

------







# 九、滑动窗口（连续重复问题）

### [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

#### A.思路:先滑动右边，大于target时加入判断res并左移左边

#### B.代码及注释：	

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int l = 0;
        int sum = 0;
        int res = Integer.MAX_VALUE;
        //移动右边界
        for (int r = 0; r < nums.length; r++) {
            sum += nums[r];
            //大于等于target时，更新res并左移
            while (sum >= target){
                res = Math.min(res,r-l+1);
                sum -= nums[l++];
            }
        }
        return res == Integer.MAX_VALUE? 0 :res;
    }
}
```

------







### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

#### A.思路:hashmap记录字符最新位置，如果出现重复，更新start

#### B.代码及注释：	

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length() == 0 ) return 0;
        //hashmap用于保存字符-出现的index
        HashMap<Character,Integer> map = new HashMap<>();
        //res记录最长值，start记录起始index
        int res = 0;
        int start = 0;
        //字符串转化为数组方便读取
        char[] chars = s.toCharArray();
        //向右滑动end
        for (int end = 0; end < s.length(); end++) {
            char c = chars[end];
            //map包含该字符时，重新设定起始边界
            if(map.containsKey(c)){
                //start为该字符的位置+1和当前左边界的最大值
                start = Math.max(map.get(c)+1,start);
            }
            //更新res
            res = Math.max(res,end-start+1);
            //更新字符最新的位置
            map.put(c,end);
        }
        return res;
    }
}
```

------

### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

#### A.思路：两两指针移动比较，求出公共前缀。依次俩俩比较。String Prefix（String strs1，String strs2）

#### B.代码及注释：

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        //初始值即为strs[0]
        String prefix = strs[0];
        for (int i = 1; i < strs.length; i++) {
            //依次求公共前缀和
            prefix = TwoPrefix(prefix,strs[i]);
            //当不存在公共部分可以提前跳出
            if(prefix.length() == 0){
                break;
            }
        }
        return prefix;
    }

    //俩俩比较求出公共前缀和
    private String TwoPrefix(String str1, String str2) {
        //len去较小的
        int len = Math.min(str1.length(),str2.length());
        //移动到不等，记录i
        int i = 0;
        for (; i < len; i++) {
            if(str1.charAt(i) != str2.charAt(i)) break;
        }
        //返回公共前缀
        return str1.substring(0,i);
    }
}
```

------

### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

#### A.思路：滑动窗口l，r。needMap记录需求字符集，count判断是否全需求，start，res记录最终结果；先移动r使窗口全包含，然后移动l，使满足条件窗口最小；判断最终结果，记录。移动l，排出需求字符，继续移动r循环。

#### B.代码及注释：

```java
class Solution {
    public String minWindow(String s, String t) {
        //need字符集，index为asc码值，>0表示需要
        int[] needMap = new int[128];
        for (char c : t.toCharArray()) {
            needMap[c]++;
        }
        //左右边界
        int l =0,r=0;
        //需求字符个数
        int count = t.length();
        //结果集的开始index
        int start = 0;
        //初始化结果res = Integer.MAX_VALUE
        int res = Integer.MAX_VALUE;
        //遍历,先滑动右边界，直到包含所有字符。
        while (r<s.length()){
            //r对应的当前字符
            char c = s.charAt(r);
            //在需求字符类，则count-1
            if(needMap[c] > 0){
                count--;
            }
            //该字符对应map-1
            needMap[c]--;
            //count==0，已经覆盖了，可以移动左边界了
            if(count == 0){
                //l对应字符不再需求之类
                while (l<r && needMap[s.charAt(l)]<0){
                    needMap[s.charAt(l)]++;
                    l++;
                }
                //此时为符合结果的最小窗口，对比最终结果
                if(r-l+1< res){
                    res =r-l+1;
                    start = l;
                }
                //移动l，继续循环移动r++
                needMap[s.charAt(l)]++;
                l++;
                count++;
            }
            r++;
        }
        return res ==Integer.MAX_VALUE? "":s.substring(start,start+ res);
    }
}
```



# 十、回溯

### [77. 组合](https://leetcode-cn.com/problems/combinations/)

#### A.思路：回溯设置res，path；设置index避免重复，终止条件为path.size==k,for循环为横向遍历从index开始，递归为纵向遍历，从当前add的i+1开始。

#### B.代码及注释：

```java
class Solution {
    //标准回溯，一个结果集res，一个路径path
    List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> combine(int n, int k) {
        LinkedList<Integer> path = new LinkedList<>();
        backtrack(n,k,1,path);
        return res;
    }
    //index用于避免重复记录
    private void backtrack(int n, int k, int index,LinkedList<Integer> path) {
        //回溯终止条件
        if(path.size() == k) {
            res.add(new LinkedList<>(path));
            return;
        }
        //for循环为横行遍历，每次[index,n]
        for (int i = index; i <= n; i++) {
            path.add(i);
            //递归为纵向遍历，每次起始为i+1
            backtrack(n,k,i+1,path);
            //回溯
            path.removeLast();
        }
    }
}
```

------

### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

#### A.思路：回溯设置res，path；设置index记录开始位置，终止条件为target==0,for循环为横向遍历从index开始，递归为纵向遍历，从当前add的i开始。

#### B.代码及注释：

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        LinkedList<Integer> path = new LinkedList<>();
        backtrack(candidates,target,0,path);
        return res;
    }
    //回溯传入target判断终止，index记录遍历开始
    private void backtrack(int[] candidates, int target,int index,LinkedList<Integer> path) {
        if(target == 0){
            res.add(new LinkedList<>(path));
            return;
        }
        for (int i = index; i < candidates.length; i++) {
            if(candidates[i] > target) break;
            path.add(candidates[i]);
            //下一次递归，还是从i开始
            backtrack(candidates,target-candidates[i],i,path);
            path.removeLast();
        }
    }
}
```

------

### [78. 子集](https://leetcode-cn.com/problems/subsets/)

#### A.思路：回溯设置res，path；设置index记录开始位置，终止条件为index==len,for循环为横向遍历从index开始，递归为纵向遍历，从当前add的i+1开始。

#### B.代码及注释：

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();


    public List<List<Integer>> subsets(int[] nums) {
        LinkedList<Integer> path = new LinkedList<>();
        backtrack(nums,0,path);
        return result;
    }

    private void backtrack(int[] nums,int index,LinkedList<Integer> path){
        //把当前path加入res即可
        result.add(new LinkedList<>(path));
        //当start==len时终止
        if(index >=nums.length) return;
        for (int i = index; i < nums.length; i++) {
            path.add(nums[i]);
            //从i+1开始递归
            backtrack(nums,i+1,path);
            path.remove(path.size() - 1);
        }
    }
}
}
```

------



### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

#### A.思路：回溯设置res，path；设置boolean[] used表示是否出现过；终止条件为path.size=len，横向for循环遍历每次从1开始，但需要加入used判断，是否continue跳过。没用过则加入path，且used[i]置为true，递归，然后回溯删除最后一个，并且置为false。

#### B.代码及注释：

```java
class Solution {
    List<List<Integer>> res = new LinkedList<>();
    public List<List<Integer>> permute(int[] nums) {
        if(nums.length == 0) return res;

        LinkedList<Integer> path = new LinkedList<>();
        //数组used表示是否使用过
        boolean[] used = new boolean[nums.length];
        backtrack(nums,used,path);
        return res;
    }

    private void backtrack(int[] nums,boolean[] used,LinkedList<Integer> path) {
        //终止条件为path的size为数字个数
        if(path.size() == nums.length){
            res.add(new LinkedList<>(path));
            return;
        }
        //横向for循环遍历，每次都从0开始
        for (int i = 0; i < nums.length; i++) {
            //如果该点被用过则跳过
            if(used[i]) continue;
            //使用将used置为true
            used[i] = true;
            //处理将nums[i]加入path
            path.add(nums[i]);
            //递归
            backtrack(nums,used,path);
            //回溯删掉最后一个
            path.removeLast();
            //将处理的used设为false
            used[i] = false;
        }
    }
}
```

------



### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

#### A.思路：回溯设置res，path；终止条件为左右为空，满足条件加入res，return,左不为空继续递归左节点，回溯移除path最后一个。右同理

#### B.代码及注释：

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();


    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        if(root == null) return res;
        //深度搜索+回溯
        LinkedList<Integer> path = new LinkedList<>();
        dfs(root,targetSum,path);
        return res;
    }

    private void dfs(TreeNode root, int targetSum,LinkedList<Integer> path) {
        //回溯的终止条件，左右为空，目标=val，path加入res否则return
        path.add(root.val);
        if(root.left == null && root.right == null){
            if(targetSum-root.val == 0){
                res.add(new LinkedList<>(path));
            }
            return;
        }
        //左不为空，dfs左
        targetSum -= root.val;
        if(root.left!=null) {
            dfs(root.left, targetSum,path);
            //回溯
            path.remove(path.size() - 1);
        }
        if(root.right!=null) {
            dfs(root.right, targetSum,path);
            path.remove(path.size() - 1);
        }
    }
```

------

### [784. 字母大小写全排列](https://leetcode-cn.com/problems/letter-case-permutation/)

#### A.思路：回溯设置res，path这里用stringBuilder；设置index遍历；终止条件为长度一致，path加入res，return；横向for遍历从index，判断c为数字直接回溯，为字母则分两种回溯。

#### B.代码及注释：



```java
class Solution {

    private List<String> res = new ArrayList<>();
    public List<String> letterCasePermutation(String s) {
        StringBuilder path = new StringBuilder();
        backtrack(s,0,path);
        return res;
    }

    private void backtrack(String s, int index, StringBuilder path) {
        if(path.length() == s.length()){
            res.add(path.toString());
            return;
        }

        for (int i = index; i < s.length(); i++) {
            char c = s.charAt(i);
            if(Character.isDigit(c)){
                path.append(c);
                backtrack(s,i+1,path);
                path.deleteCharAt(path.length()-1);
            }else {
                char A = Character.toUpperCase(c);
                path.append(A);
                backtrack(s,i+1,path);
                path.deleteCharAt(path.length()-1);

                char a = Character.toLowerCase(c);
                path.append(a);
                backtrack(s,i+1,path);
                path.deleteCharAt(path.length()-1);
            }
        }
    }
}
```





### [93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

#### A.思路：回溯设置res，path这里直接在原s上更改更简单；设置index避免重复，设置pointNum判断结束；终止条件为pointNum=3，剩余数字满足条件加入res，return；横向for遍历从index到i，判断是否合法，为s的[0,i+1]后加入.，下次起始为i+2，回溯移除i+1后的.。

#### B.代码及注释：

```java
class Solution {
    List<String> res = new ArrayList<>();
    //这里直接在原字符串操作会更简单
    //StringBuilder path = new StringBuilder();
    public List<String> restoreIpAddresses(String s) {
        //这里设置一个pointNum来判断结束条件
        backtrack(s,0,0);
        return res;
    }

    private void backtrack(String s,int index,int pointNum){
        //当加入三个点时，结束
        if(pointNum == 3){
            //如果index到剩下的数字符合规范，则加入res
            if(isValid(s,index,s.length()-1)){
                res.add(s);
            }
            //否则return
            return;
        }
        //横向for循环，从index找到i
        for (int i = index; i < s.length(); i++) {
            //判断index-i是否合法
            if(isValid(s,index,i)){
                //合法的话，给字符串加入.，
                s = s.substring(0,i+1)+"."+s.substring(i+1);
                pointNum++;
                //纵向递归，之后的坐标会+1，index会从i+2开始
                backtrack(s,i+2,pointNum);
                //回溯。减去.的数量
                pointNum--;
                //字符串减去.
                s = s.substring(0,i+1)+s.substring(i+2);
            }else {
                //不合法直接下一轮循环
                break;
            }
        }
    }
    //判断是否合法
    private boolean isValid(String s, int l, int r) {
        if(l>r) return false;
        //只允许0开头只有一个0
        if(s.charAt(l) == '0' && l != r){
            return false;
        }
        //设置结果num
        int num = 0;
        for (int i = l; i <= r; i++) {
            //不是数字也判断不合法
            if(s.charAt(i) > '9' || s.charAt(i) < '0') {
                return false;
            }
            //每次给num*10+当前位
            num = num*10+s.charAt(i)-'0';
            //如果大于255不合法
            if(num>255){
                return false;
            }
        }
        return true;
    }
}
```



------



# 十一、计算（进位、计算器）

### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

#### A.思路：缺位补0，设置carry，sum=x+y+carry，新carry=sum/10，新sum=sum%10放在当前位

#### B.代码及注释：

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //头节点需要操作，设置虚拟头节点
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        
        //计算有进位，定义carry
        int carry = 0;
        //计算终止条件没有数（x，y，carry均为0）
        while (l1!=null||l2!=null||carry!=0){
            //对于空节点直接补0
            int x = l1==null? 0: l1.val;
            int y = l2==null? 0: l2.val;
            //计算sum = x+y+carry
            int sum = x+y+carry;
            //新carry = sum/10，新sum=sum%10为当前位值
            carry = sum/10;
            sum=sum%10;
            //生产新节点加到链表
            cur.next=new ListNode(sum);
            //移动cur，l1,l2需要判断是否已经走完
            cur = cur.next;
            if(l1!=null) l1 = l1.next;
            if(l2!=null) l2 = l2.next;
        }
        //虚拟头节点的返回方式
        return dummy.next;
    }
}
```

#### C.补充：[445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

不是末位开始，先反转链表再相加

------

### [165. 比较版本号](https://leetcode-cn.com/problems/compare-version-numbers/)

#### A.思路：用.判断终止位，每次初始化a，b为0，10*a+c-‘0’；比较

#### B.代码及注释：

```java
class Solution {
    public int compareVersion(String version1, String version2) {
        int i = 0, j = 0;
        int len1 = version1.length();
        int len2 = version2.length();
        while (i<len1||j<len2){
            int a = 0,b=0;
            //遇到.前用a*10+c-'0'记录当前数字
            while (i< len1 && version1.charAt(i) != '.'){
                a = a*10+version1.charAt(i) - '0';
                i++;
            }
            while (j< len2 && version2.charAt(j) != '.'){
                b = b*10+version2.charAt(j) - '0';
                j++;
            }
            if(a>b) return 1;
            else if(a<b) return -1;
            i++;
            j++;
        }
        return 0;
    }
}
```

------







### [415. 字符串相加（大数相加）](https://leetcode-cn.com/problems/add-strings/)

#### A.思路：缺位补0，设置carry，sum=x+y+carry，新carry=sum/10，新sum=sum%10放在当前位

#### B.代码及注释：

```java
class Solution {
    public String addStrings(String num1, String num2) {
        //初始化结果值res
        StringBuilder res = new StringBuilder("");
        //定义两数指针，从末尾开始加
        int i = num1.length()-1;
        int j = num2.length()-1;
        //初始化carry=0
        int carry = 0;
        while (i>=0||j>=0||carry!=0){
            //取n1,n2的值
            int n1 = i>=0? num1.charAt(i) - '0':0;
            int n2 = j>=0? num2.charAt(j) - '0':0;
            //加减的固定写法
            int temp = n1 + n2 + carry;
            carry = temp/10;
            res.append(temp%10);
            i--;
            j--;
        }
        return res.reverse().toString();
    }
}
```

------

### [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

#### A.思路：指针移动，先去空格，然后判断符号位，输入数字num=c-‘0’，每一轮res=res\*10+sign\*num,加入前先判断res。即

#### res\*10+sign\*num>Max->(res>Max/10||(res=Max/10&&num>Max%10)时返回

#### B.代码及注释：

```java
class Solution {
    public int myAtoi(String s) {
        //初始化结果res
        int res = 0;
        //字符指针与符号位
        int i = 0,sign=1;
        //去除前导空格
        while (i<s.length() && s.charAt(i) == ' '){
            i++;
        }
        //用start判断是否i为符号位
        int start =i;
        for (;i<s.length();i++){
            char c = s.charAt(i);
            //如果i为符号位，且有符号则改变sign
            if(i == start && c == '+'){
                sign = 1;
            }else if(i == start&&c == '-'){
                sign = -1;
                //判断是否为数字
            }else if ('0'<=c&&c<='9'){
                //记录该数字为num
                int num = c - '0';
                //给res加入高位时判断即res*10+sign*num>Max||res*10+sign*num<Min
                if(res > Integer.MAX_VALUE/10||(res == Integer.MAX_VALUE/10&&num>Integer.MAX_VALUE%10))
                    return Integer.MAX_VALUE;
                else if(res <Integer.MIN_VALUE/10||(res==Integer.MIN_VALUE/10&&-num<Integer.MIN_VALUE%10))
                    return Integer.MIN_VALUE;
                //每次都是res*10+sign*num
                res = res*10+sign*num;
            }else {
                break;
            }
        }
        return res;
    }
}
```

------

### [224. 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

#### A.思路：遇num记录num，遇+-计算res，重置num，更新sign；遇（压res和sign入栈，并重置；遇）计算res，stack依次弹出符号*，上个数字+，重置num。

#### B.代码及注释：

```java
class Solution {
    public int calculate(String s) {
        //计算结果
        int res = 0;
        //当前数字
        int num = 0;
        //符号
        int sign = 1;
        //利用辅助栈，存储结果
        Stack<Integer> stack = new Stack<>();

        //遍历字符串
        char[] chars = s.toCharArray();
        int len = s.length();
        for (int i = 0; i < len; i++) {
            char c = chars[i];
            //当c为数字，更新num
            if(c>='0'&&c<='9'){
                num = 10*num+c-'0';
            //c为符号，表示前面的数字可以记录res了，还原num=0，并且记录下一个数字的符号
            }else if(c == '+'||c=='-'){
                res += sign*num;
                num = 0;
                //更新sign
                sign = c=='+'? 1:-1;
            //遇到左括号时表示需要先计算后面的部分，把之前的res和sign记录下来。重置
            }else if(c=='('){
                stack.add(res);
                stack.add(sign);
                //重置res和sign
                res = 0;
                sign = 1;
            //遇到右括时，跟遇到符号一样，先计算完res，还原num,并在stack中弹出res的符号，和上一个值
            }else if(c==')'){
                res += sign*num;
                num = 0;
                res *= stack.pop();
                res += stack.pop();
            }
        }
        //最后一个num也得加上
        res += sign*num;
        return res;
    }
}
```

------

### [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)

#### A.思路：遇num记录num，遇字母计算res，遇【压num和res入栈，并重置；遇】计算num*res+pop

```java
class Solution {
    public String decodeString(String s) {
        int num = 0;
        StringBuilder res = new StringBuilder();
        Stack<Integer> num_stack = new Stack<>();
        Stack<String> res_stack = new Stack<>();

        //遍历
        char[] chars = s.toCharArray();
        for (char c : chars) {
            if(c>='0' &&c<='9'){
                num = 10*num+(c-'0');
            } else if(c>='a'&&c<='z'){
                res.append(c);
            }else if(c=='['){
                num_stack.add(num);
                res_stack.add(res.toString());
                num = 0;
                res = new StringBuilder();
            }else if(c==']'){
                int multi = num_stack.pop();
                StringBuilder temp = new StringBuilder();
                for (int i = 0; i < multi; i++) {
                    temp.append(res);
                }
                res = new StringBuilder(res_stack.pop()+temp);
            }
        }
        return res.toString();
    }
}
```

------



# 十二、单调栈（队列）问题

### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

#### A.思路：使用一个helper辅助栈，只有小的在加入

#### B.代码及注释：

```java
class MinStack {
    //data为记录数据的普通栈
    private Stack<Integer> data;
    //helper维护最小栈
    private Stack<Integer> helper;

    /** initialize your data structure here. */

    public MinStack() {
        data = new Stack<>();
        helper = new Stack<>();
    }
    
    public void push(int val) {
        data.add(val);
        //helper栈加入时只加入比栈顶小的
        if(helper.isEmpty()||val<= helper.peek()){
            helper.add(val);
        }
    }
    
    public void pop() {
        int pop = data.pop();
        //相当时才弹出
        if(pop == helper.peek()){
            helper.pop();
        }
    }
    
    public int top() {
        return data.peek();
    }
    
    public int getMin() {
        return helper.peek();
    }
}
```

------

### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

#### A.思路：单调递减栈，需要从头取元素，用双端队列。破坏递减性时每次加入队列从队尾移除比它小的再加入即可，然后每次判断是否需要移除从头部拿。

#### B.代码及注释：

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        //初始化结果集为int[len-k+1]
        int[] res = new int[nums.length-k+1];
        //维护一个单调递减队列，利用双端队列；即当大值来的时候直接移除比他小的部分
        LinkedList<Integer> queue = new LinkedList<>();
        //循环遍历数组
        for (int i = 0; i < nums.length; i++) {
            //当数组不为空，移除比其小的。
            while (!queue.isEmpty()&&nums[queue.peekLast()]<=nums[i]){
                queue.pollLast();
            }
            //入队
            queue.addLast(i);
            //当peek==i-k时，表示滑动窗口已经不再包含
            if(queue.peek()== i -k){
                queue.poll();
            }
            //当i>k-1时开始更新结果，一直到len
            if(i>=k-1){
                res[i+1-k] = nums[queue.peek()];
            }
        }
        return res;
    }
}
```

------



### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

#### A.思路：单调递减栈，破坏递减性时需要拿出栈顶，得到栈顶的结果

#### B.代码及注释：

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int[] res = new int[temperatures.length];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < temperatures.length; i++) {
            //当单调递减性被破坏时，弹出的天数表示找了结果i-pre
            while (!stack.isEmpty()&&temperatures[i]>temperatures[stack.peek()]){
                int pre = stack.pop();
                res[pre] = i - pre;
            }
            stack.push(i);
        }
        return res;
    }
}
```

------

### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

#### A.思路：单调递减栈，破坏递减性时需要拿出栈顶接雨水，h取左右最小值，w取宽度。

#### B.代码及注释：

```java
class Solution {
    public int trap(int[] height) {
        int res = 0;
        //维护一个单调栈
        Stack<Integer> stack = new Stack<>();
        //遍历height数组
        for (int i = 0; i < height.length; i++) {
            //当栈有元素且要入一个比栈顶大的元素时，开始接雨水
            while (!stack.isEmpty() && height[i]>height[stack.peek()]){
                int popNum = stack.pop();
                //对于重复的元素，直接弹出跳过
                while (!stack.isEmpty()&&height[popNum]==height[stack.peek()]){
                    stack.pop();
                }
                //只有该pop不是[0，i）最大开始接雨水
                if(!stack.isEmpty()){
                    //取左右最短的
                    int h = Math.min(height[stack.peek()],height[i])-height[popNum];
                    //宽度
                    int w = i - stack.peek()-1;
                    res += w*h;
                }
            }
            stack.push(i);
        }
        return res;
    }
}
```



------



### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

#### A.思路：单调递增栈，破坏递减性时需要拿出栈顶构造矩形，h取当前值，w取宽度。

#### B.代码及注释：

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int res = 0;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < heights.length; i++) {
            //当单调递增性被破坏时
            while (!stack.isEmpty()&&heights[i]<heights[stack.peek()]){
                //弹出的元素最大矩阵可以计算了
                int popNum = stack.pop();
                //重复的可以直接跳过肯定不为最大
                while (!stack.isEmpty()&&heights[stack.peek()]==heights[popNum]){
                    stack.pop();
                }
                int w;
                //如果栈为空，说明该矩阵为[0,i）最小
                if(stack.isEmpty()){
                    w = i;
                }else {
                    //否则为两矩阵中间
                    w = i - stack.peek()-1;
                }
                res = Math.max(res,w*heights[popNum]);
            }
            stack.push(i);
        }
        //最后栈里为递增序列，也要相同判断
        while (!stack.isEmpty()){
            int popNum = stack.pop();
            while (!stack.isEmpty()&&heights[stack.peek()]==heights[popNum]){
                stack.pop();
            }
            int w;
            //为空时说明为全序列最小
            if(stack.isEmpty()){
                w = heights.length;
            }else {
                w = heights.length - stack.peek()-1;
            }
            res = Math.max(res,w*heights[popNum]);
        }
        return res;
    }
}
```

------

### [402. 移掉 K 位数字](https://leetcode-cn.com/problems/remove-k-digits/)

#### A.思路：当不为空时或者为空时不为0可以push，遇到比peek小的时候弹出时减少k

#### B.代码及注释：

```java
class Solution {
    public String removeKdigits(String num, int k) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : num.toCharArray()) {
            //当不为空，且比栈顶小时，给k--
            while (k>0 && !stack.isEmpty() && c < stack.peek()){
                stack.pop();
                k--;
            }
            if(c != '0' || !stack.isEmpty()){
                stack.push(c);
            }
        }
        while (k>0 && !stack.isEmpty() ){
            stack.pop();
            k--;
        }
        StringBuffer res = new StringBuffer();
        while (!stack.isEmpty()){
            res.append(stack.removeLast());
        }

        return res.length() == 0? "0":res.toString();

    }
}
```





# 十三、贪心算法

### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

#### A.思路：保存之前股票的最小值

#### B.代码及注释：

```java
class Solution {
    public int maxProfit(int[] prices) {
        //贪心算法初始化为目前的最低价
        int minPrice = prices[0];
        int res = 0;
        for (int i = 1; i < prices.length; i++) {
            //记录最低价
            if(prices[i] < minPrice) minPrice = prices[i];
            //如果利润大于res更新res
            else if(prices[i] - minPrice > res){
                res = prices[i] - minPrice;
            }
        }
        return res;
    }
}
```

------

### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

#### A.思路：每次比前一天价格高就卖出，获得收益

#### B.代码及注释：

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            int temp = prices[i] -prices[i-1];
            if(temp>0) profit+=temp;
        }
        return profit;
    }
}
```

------

### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

#### A.思路：每次取最大的cover范围 i+nums[i]

#### B.代码及注释：

```java
class Solution {
    public boolean canJump(int[] nums) {
        int max = nums[0];
        for (int i = 0; i <= max; i++) {
            max = Math.max(max,i+nums[i]);
            if(max>= nums.length-1) return true;
        }
        return false;
    }
}
```

------

### [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

#### A.思路：每次取最大的cover范围 i+nums[i],设立step和reach变量，当i遍历到reach时，更新reach为max，step++

#### B.代码及注释：

```java
class Solution {
    public int jump(int[] nums) {
        if(nums.length== 1) return 0;
        int max = nums[0];
        int step = 0;
        int reach = 0;
        for (int i = 0; i <= max; i++) {
            max = Math.max(max,i+nums[i]);
            if(max>= nums.length-1) return step+1;
            if(i==reach){
                step++;
                reach = max;
            }
        }
        return step;
    }
}
```

------



### [470. 用 Rand7() 实现 Rand10()](https://leetcode-cn.com/problems/implement-rand10-using-rand7/)

#### A.思路：(RandX()-1)*X+randX-----[1,X]->[1,X^2]

#### B.代码及注释：

```java
class Solution extends SolBase {
    public int rand10() {
        while (true){
            //rand10可以用%10+1，现在只需要生成超过
            //rand7->[1,7],(rand7-1)*10+rand7->[1,49]
            int num = (rand7()-1)*7+rand7();
            //生成数的范围[1,40]时
            if(num <= 40) return num%10+1;
        }
    }
}
```

# 十四、位运算

### [201. 数字范围按位与](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

#### A.思路：找到二进制公共前缀（1.右移直到两数相等，再左移还原；2.n&(n-1)去除最右的1

#### B.代码及注释：

```java
class Solution {
    //找公共前缀
    public int rangeBitwiseAnd(int left, int right) {
        //方法1：左移直到相等，在还原
        /*int n = 0;
        while (left != right){
            left >>=1;
            right >>=1;
            n++;
        }
        return left<<n;*/
        //方法2：n&(n-1)
        while (left < right){
            right = right&(right-1);
        }
        return right;
    }
}
```

------

### [191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

#### A.思路：n&(n-1)去除最右的1

#### B.代码及注释：

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while (n!=0){
            n = n&(n-1);
            res++;
        }
        return res;
    }
}
```

------

### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

#### A.思路：摩尔投票法，当count=0时更新res

#### B.代码及注释：

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int res = nums[0];
        for (int num : nums) {
            //当count = 0时更新res
            if(count == 0){
                res = num;
            }
            if(num == res){
                count++;
            }else {
                count--;
            }
        }
        return res;
    }
}
```

------

### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

#### A.思路：degrees维护级别，遍历，degrees++，用list存入i对应的list（孩子）；遍历当degrees==0入队，然后弹出时num--。找到pre的孩子--，为0时入队

#### B.代码及注释：

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] degrees = new int[numCourses];
        List<List<Integer>> list = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();

        //事先创建空的list
        for (int i = 0; i < numCourses; i++) {
            list.add(new ArrayList<>());
        }
        //
        for (int[] cp : prerequisites) {
            degrees[cp[0]]++;//出现一次前置，级别+1
            list.get(cp[1]).add(cp[0]);//创建连接表，cp[1]的前置为cp【0】
        }
        for (int i = 0; i < numCourses; i++) {
            if(degrees[i] == 0)
                queue.add(i);//级别为0的入队
        }
        while (!queue.isEmpty()){
            //每弹出一个
            int pre = queue.poll();
            numCourses--;
            for (Integer cur : list.get(pre)) {
                if(--degrees[cur] == 0){
                    queue.add(cur);
                }
            }
        }
        return numCourses==0;
    }
}
```

------





# 十五、岛屿问题

### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

#### A.思路：递归深搜，每次res++

#### B.代码及注释：

```java
class Solution {
    public int numIslands(char[][] grid) {
        int res = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                //每次找到新大陆，递归全部置为非1
                if(grid[i][j] == '1'){
                    dfs(grid,i,j);
                    res++;
                }
            }
        }
        return res;
    }

    //递归深搜，目的将岛屿全置为2
    private void dfs(char[][] grid, int i, int j) {
        //终止条件超过边界或者不为1
        if(i<0||j<0||i>=grid.length||j>=grid[0].length||grid[i][j] != '1') return;
        //遍历过的将其置为2
        grid[i][j] = '2';
        //dfs其周围
        dfs(grid,i+1,j);
        dfs(grid,i,j+1);
        dfs(grid,i-1,j);
        dfs(grid,i,j-1);
    }
}
```

------

### [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

#### A.思路：递归深搜，每次返回最大值，顺便置为0

#### B.代码及注释：

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int res = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == 1){
                    res = Math.max(res,dfs(i,j,grid));
                }
            }
        }
        return res;
    }

    private int dfs(int i, int j, int[][] grid) {
        if(i<0||j<0||i>=grid.length||j>=grid[i].length||grid[i][j]==0){
            return 0;
        }
        grid[i][j] = 0;
        return 1+dfs(i+1,j,grid)+dfs(i-1,j,grid)+dfs(i,j-1,grid)+dfs(i,j+1,grid);
    }
}
```

------



### [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

#### A.思路：递归深搜，判断全局变量contain

#### B.代码及注释：

```java
class Solution {
    boolean isContain;

    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        char first = word.charAt(0);
        boolean[][] used = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if(board[i][j] == first){
                    //dfs深搜
                    dfs(board,i,j,word,0,used);
                    if(isContain){
                        return true;
                    }
                }
            }
        }
        return false;
    }

    private void dfs(char[][] board, int m, int n, String word, int index, boolean[][] used) {
        //终止条件为index = word.length
        if(index == word.length()){
            isContain = true;
            return;
        }
        //当越界或者当前元素非word的index或者used已经使用了
        if(!isInArea(board,m,n) || board[m][n] != word.charAt(index) || used[m][n]){
            return;
        }
        //匹配成功时，将used设为true，进行下一个匹配
        used[m][n] = true;

        dfs(board,m,n-1,word,index+1,used);
        dfs(board,m,n+1,word,index+1,used);
        dfs(board,m-1,n,word,index+1,used);
        dfs(board,m+1,n,word,index+1,used);
        used[m][n] = false;
    }

    private boolean isInArea(char[][] board, int m, int n) {
        return m>=0 && m< board.length && n>=0 && n< board[0].length;
    }
}
```

## 
