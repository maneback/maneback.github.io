---
title: 树
date: 2021-09-09 13:31:55
tags: 
  - tree
  - Leetcode
categories: Algorithm

---

不同的二叉搜索树

有序链表转二叉树

二叉树展开成链表

填充每个节点的下一个右侧节点指针

具有所有最深节点的最小子树

二叉树的最近公共祖先

<!--more-->

#### 不同的二叉搜索树

题目链接：

[LeetCode-96](https://leetcode-cn.com/problems/unique-binary-search-trees/)

[LeetCode-95](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

##### 题目描述

> 给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。
>
> 给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

##### 分析

这两个题基本上雷同，所以就放在一起了。

树嘛，由树一定要想到递归操作。对于任意一个整数`1<=i<=n`， 把它作为根节点都能来构造二叉搜索树，并递归地用`[1, i-1]`和 `[i+1, n]`来构造左右子树。当数组中只有一个数字或零个数字时，则说明到了叶子结点，停止构造，再返回给父节点。这是一个自底向上的过程，只有拿到了子树，才能构造当前节点，所以是一个后序遍历的过程。

对于数组`[l,r]`总结一下递归中每一步的操作：

1. 如果数组中只含有零个或一个元素，则直接返回一个空节点/叶子节点。
2. 对于任一一个数字`l<=i<=r`， 以`i`为根节点，用`[l, i-1]`和 `[i+1, r]`来构造左右子树。
3. 分别返回左右子树的数量`ls`和 `rs`（列表`left_childs`和`right_childs`）。
4. 该节点的子树数量为`ls*rs`。 （以每一个左右子树对，构造一个二叉树）。
5. 返回树的数量（返回树的数组）。

##### 代码

- 构造树

```java
public List<TreeNode> generateTrees(int n) {
    if (n == 0) {
        return new LinkedList<TreeNode>();
    }
    return generateTrees(1, n);
}
public List<TreeNode> generateTrees(int l, int r){
    List<TreeNode> trees = new ArrayList<>();
    if(l>r){
        trees.add(null);
        return trees;
    }
    for(int root=l; root<=r; root++){
        List<TreeNode> left_child = generateTrees(l, root-1);
        List<TreeNode> right_child = generateTrees(root+1, r);

        for(TreeNode left: left_child){
            for(TreeNode right: right_child){
                TreeNode node = new TreeNode(root);
                node.left = left;
                node.right = right;
                trees.add(node);
            }
        }
    }
    return trees;
}
```

- 数量

```java
public int numTrees(int n) {
    if (n == 0) {
        return 0;
    }
    return count(1, n);
}
public int count(int l, int r){
    int res = 0;
    if(l>r){
        return 1;
    }
    for(int root=l; root<=r; root++){
        int left = generateTrees(l, root-1);
        int right = generateTrees(root+1, r);
        
        res += left * right;

    }
    return res;
}
```

#### 有序链表转二叉树

题目链接 [LeetCode-109](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

##### 题目描述

> 给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。
>
> 本题中，一个**高度平衡二叉树**是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

##### 分析

第一种方法，我们可以先遍历一遍链表，然后把链表中的值存在一个数组中，再用数组来构建一个平衡二叉搜索树。这种方法比较简单，而且需要开辟额外的空间，所以不再赘述。

第二种方法，可以采用递归。对于子链表`[left, right]`，每次使用快慢指针找到中点来构建。

第二种方法的时间复杂度主要在于反复地寻找中位数节点。由于二叉树的中序遍历结构就是链表本身，所以我们可以将分治过程与中序遍历结合起来：

具体做法就是，在构造过程中，不用先找出链表中的中位数节点，只需要找到它的序号，并预先构造一个占位节点。用一个外部指针去遍历链表，每构造一个节点，该指针都向后移动一个位置。当中序遍历构造完左子树之后，再填充节点的正确的值。

##### 代码

```java
class Solution{
    ListNode globalHead;
    public TreeNode sortedListToBST(ListNode head){
        globalHead = head;
        int length = listLength(head);
        return buildTree(0, length-1);
    }

    public TreeNode buildTree(int left, int right){
        if(left>right){
            return null;
        }
        int mid = (left+right+1)/2;
        TreeNode root = new TreeNode();
        root.left = buildTree(left, mid-1);
        root.val = globalHead.val;
        globalHead = globalHead.next;
        root.right = buildTree(mid+1, right);
        return root;
    }

    public int listLength(ListNode head){
        int len = 0;
        while(head!=null){
            head = head.next;
            len++;
        }
        return len;
    }
}
```

#### 二叉树展开成链表

题目链接 [LeetCode-114](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

##### 题目描述

> 给你二叉树的根结点 `root `，请你将它展开为一个单链表：
>
> 展开后的单链表应该同样使用 `TreeNode `，其中 right 子指针指向链表中下一个结点，而左子指针始终为 `null `。
> 展开后的单链表应该与二叉树 **先序遍历** 顺序相同。

##### 分析

第一种简单做法，是先用一次先序遍历，把节点都保存在都保存在一个列表中，再给它们的左右子树赋值。

第二种做法是，遍历每一个节点。对于一个节点，如果它的左子树为空，则说明当前节点不需要操作，直接去操作它的右节点。

如果左子树非空，则需要以下三个步骤：

①找到先序遍历时左子树的最后一个节点 `pre`。（`curr`**左子树中的最右节点**）

②然后把 `curr` 的右子树连接到`pre` 的右子树。

③把`curr`的左子树连接给 `curr` 的右子树。

##### 代码

```java
class Solution{
	public void flatten(TreeNode root){
        TreeNode cur = root;
        while(cur!=null){
            if(cur.left==null){
                cur = cur.right;
                continue;
            }
            //找到前驱节点
            TreeNode pre = cur.left;
            while(pre.right!=null){
                pre = pre.right;
            }
            pre.right = cur.right;
            cur.right = cur.left;
            cur.left = null;
        }
    }
}
```

#### 填充每个节点的下一个右侧节点指针 II

题目链接 [LeetCode-117](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

##### 题目描述

> 给定一个二叉树
>
> ```
> struct Node {
> int val;
> Node *left;
> Node *right;
> Node *next;
> }
> ```
>
> 填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。
>
> 初始状态下，所有 next 指针都被设置为 `NULL`。
>
> **你只能使用常量级额外空间。**

![image-20210905143101119](https://gitee.com/MyTypora/typorapic/raw/master/img/20210905143101.png)

##### 分析

如果不限制空间的话，那么使用层次遍历就可以解决了。无法使用额外空间的话，其实对于每一层的节点，可以用 `next` 指针来代替队列的作用。



##### 代码

```java
class Solution{
    Node last=null;
    Node nextLevelStart = null;// 下一层的起始节点
    public Node connect(Node root){
        if(root==null){
            return null;
        }
        Node start = root;
        while(start!=null){
            last = null;
            nextLevelStart = null;
            for(Node p = start; p.next!=null; p=p.next){
                if(p.left!=null){
                    handle(p.left);
                }
                if(p.right!=null){
                    handle(p.right);
                }
            }
            start = nextLevelStart;
        }
        return root;
    }
    public void handle(Node p){
        if(last!=null){
            last.next = p;
        }
        if(nextLevelStart == null){
            nextLevelStart = p;
        }
        last = p;
    }
}
```

#### 具有所有最深节点的最小子树

题目链接：[LeetCode-865](https://leetcode-cn.com/problems/smallest-subtree-with-all-the-deepest-nodes/)

##### 题目描述：

> 给定一个根为 `root `的二叉树，每个节点的深度是 该节点到根的最短距离 。
>
> 如果一个节点在 **整个树** 的任意节点之间具有最大的深度，则该节点是 **最深的** 。
>
> 一个节点的 **子树** 是该节点加上它的所有后代的集合。
>
> 返回能满足 **以该节点为根的子树中包含所有最深的节点** 这一条件的具有最大深度的节点。

##### 分析

首先考虑计算每个节点的深度，很容易采用后续遍历的方式，自底向下地来计算。

那么再进一步考虑：能不能在计算深度的**同时**采用一次遍历也返回具有最大深度的节点呢？

这时候就要采用**递归**和**局部**的思想：

- 如果左右子树深度相同，那么该节点就是最小子树的根节点，返回该节点以及该节点的深度。
- 如果左子树更深，那么最小子树的根节点就在左子树中，且就是递归中左子树返回的节点。
- 如果右子树更深，那么最小子树的根节点就在右子树中，且就是递归中右子树返回的节点。

由于递归要返回两部分内容，我们就需要使用另外一个类，或数据结构来表示返回的结果。一个域是节点类型，表示最小子树的根节点，另一个域是整数类型，表示子树的深度。

##### 代码

```java
class Solution{
	public TreeNode subtreeWithAllDeepest(TreeNode root) {
        return subdfs(root).node;
    }
    public Res subdfs(TreeNode node){
        if(node == null) return new Res(null, 0);
        Res L = subdfs(node.left);
        Res R = subdfs(node.right);
        if(L.height>R.height) return new Res(L.node, L.height+1);
        else if(R.height>L.height) return new Res(R.node, R.height+1);
        return new Res(node, L.height+1);
    }
}
class Res{
    TreeNode node;
    int height;
    Res(TreeNode n, int h){
        node = n;
        height = h;
    }
}
```

#### 二叉树的最近公共祖先

题目链接 [LeetCode-236](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 给定一个二叉树，找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

##### 分析

对于寻找最近公共祖先，有两种情况：

1. p 在 q 的子树中，最近公共祖先即为 q。
2. p, q 在分别某一个节点 node 的左右子树中。

因此，我们可以使用后序遍历：

首先判断 node 的值是否与 p 或 q 相等，若相等，或者遇到node 为空节点，则直接返回 node。这是递归的边界条件。

若不相等再递归地查找 p 或者 q 是否在 node 的左子树或右子树中，如果在的话，返回节点p或者q，不在的话返回空。设左右子树递归返回的结果为 left 和 right。

1. 若 left right 同时为空，则说明不在 node 的子树中。继续返回 空
2. 若 left 和 right 同时不为空，说明 p 和 q 分别在 node 左右子树中，返回 node 为最近祖先。
3. 若 left 为空， right 不为空，有以下三种情况：
   1. right指向 q，还未找到p。
   2. p，q 均在 node 的右子树中，且 right 指向最近公共祖先。
   3. right 指向q，且p在q 的子树中。

这里解释一下，为什么不需要两个变量来记录在子树中 **找到p** 和 **找到q** 这两件事情。

在上面的**首先** 与 **3.1** 中，如果 p 在 q 的子树中，我们是访问不到 节点 p 的。在访问到 q 的时候，递归就返回了。返回节点即为公共节点 q。这样的话对于其之上每个节点的递归，left 和 right 都一定只有一个不为空，即每次返回的都是q。

若 p 和 q 分别在某一节点的 node 中左右子树中，对于其第一个公共祖先 node。其递归的 left 和 right 一定都不为空，且一定是 left 和 right 一个指向 p， 一个指向 q。这时，我们返回的节点 node，即为最近的公共祖先。而对于 node 的祖先节点，其left 和right 也同上，一定只有一个不为空，每次向上返回的不为空的节点即为公共祖先 node。

##### 代码

```java
class Solution {
    private TreeNode ans;
    public TreeNode dfs(TreeNode root, TreeNode p, TreeNode q){
        if(root==null || root==p || root==q){
            return root;
        }
        TreeNode left = dfs(root.left, p, q);
        TreeNode right = dfs(root.right, p, q);
        if(left==null && right==null) return null; // 不在当前 root 的子树中
        if(left==null && right!=null) return right; // 至少有一个在右子树中
        if(left!=null && right==null) return left;
        return root;
    }
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        return dfs(root, p, q);
    }
}
```

