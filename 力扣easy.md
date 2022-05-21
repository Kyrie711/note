# 数据结构

## 树



### 深度优先遍历 dfs

深度优先遍历DFS 与树的先序遍历比较类似。

``` js
const tree = {
    name: 3,
    children: [
        {
            name: 9,
            children: []
        },
        {
            name: 20,
            children: [
                {
                    name: 15,
                    children: []		
                },
                {
                    name: 7,
                    children: []		
                }
            ]
        }
    ]
}

/*深度优先遍历三种方式*/
// 先序，根左右
let deepTraversal1 = (node, nodeList = []) => {
  if (node !== null) {
    nodeList.push(node)
    let children = node.children
    for (let i = 0; i < children.length; i++) {
      deepTraversal1(children[i], nodeList)
    }
  }
  return nodeList
}


let nodes = []
let deepTraversal2 = (node) => {
    if (node !== null) {
        nodes.push(node)
        let children = node.children
        for (let i = 0; i < children.length; i++) {
            deepTraversal2(children[i])
        }
    }
    return nodes
}
// 非递归
let deepTraversal3 = (node) => {
  let stack = []
  let nodes = []
  if (node) {
    // 推入当前处理的node
    stack.push(node)
    while (stack.length) {
      let item = stack.pop()
      nodes.push(item)
      let children = item.children
      // node = [] stack = [parent]
      // node = [parent] stack = [child3,child2,child1]
      // node = [parent, child1] stack = [child3,child2,child1-2,child1-1]
      // node = [parent, child1-1] stack = [child3,child2,child1-2]
      for (let i = children.length - 1; i >= 0; i--) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
```



### 广度优先遍历 bfs

``` js
const tree = {
    name: 'root',
    children: [
        {
            name: 'c1',
            children: [
                {
                    name: 'c11',
                    children: []		
                },
                {
                    name: 'c12',
                    children: []		
                }
            ]
        },
        {
            name: 'c2',
            children: [
                {
                    name: 'c21',
                    children: []		
                },
                {
                    name: 'c22',
                    children: []		
                }
            ]
        }
    ]
}

let widthTraversal2 = (node) => {
  let nodes = []
  let stack = []
  if (node) {
    stack.push(node)
    while (stack.length) {
      let item = stack.shift()
      nodes.push(item)
      let children = item.children
        // 队列，先进先出
        // nodes = [] stack = [parent]
        // nodes = [parent] stack = [child1,child2,child3]
        // nodes = [parent, child1] stack = [child2,child3,child1-1,child1-2]
        // nodes = [parent,child1,child2]
      for (let i = 0; i < children.length; i++) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
```



### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)



``` js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
// 左根右
// 循环
var inorderTraversal = function(root) {
    let res = []
    let stack = []

    while(root) {
        stack.push(root)
        root = root.left
    }

    while(stack.length) {
        let root = stack.pop()
        res.push(root.val)
        root = root.right

        while(root) {
            stack.push(root)
            root = root.left
        }
    }

    return res;
};

//递归
function dfs(res, root) {
  if (!root) return;
  dfs(res, root.left);
  res.push(root.val);
  dfs(res, root.right);
}
var inorderTraversal = function (root) {
  const res = [];
  dfs(res, root);
  return res;
};
```



### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

```` mathematica
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
 

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

进阶：

你可以运用递归和迭代两种方法解决这个问题吗？
````



``` js
给你一个二叉树的根节点 root ， 检查它是否轴对称。

解法一：递归。树为左右对称，则这棵树的左右子树必须值相等，再去判断左右子树的左右子树也要对称。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
//这个函数就是判断某个树是否左右对称
function isSame(l, r) {
  //都为空则符合
  if (!l && !r) return true;
  //否则如果是只有一个为空，肯定不符合
  if (!l || !r) return false;

  //这棵树的左右子树必须值相等，再去判断左右子树的左右子树
  if (l.val === r.val && isSame(l.left, r.right) && isSame(l.right, r.left)) {
    return true;
  }
  //否则不符合，这句一定得加上
  return false;
}
var isSymmetric = function(root) {
  if (!root) return true;
  return isSame(root.left, root.right);
};


解法二：BFS迭代。入 queue 的顺序：
左子树的左子树，右子树的右子树
左子树的右子树，右子树的左子树。
出 queue 的时候，检查两两是否对称。
var isSymmetric = function(root) {
    if(root==null)return true;
    const queue=[];
    queue.push(root.left,root.right);
    while(queue.length){
        const len=queue.length;
        for(let i=0;i<len;i+=2){
            const left=queue.shift();
            const right=queue.shift();
            
            if(left&&right){
                if(left.val!=right.val)return false;
                queue.push(left.left,right.right);
                queue.push(left.right,right.left);
            }
            if (!left && !right) {
                continue
            }
            if(!left || !right){
                return false;
            }      
        }
    }
    return true;
};
```





### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

``` mathematica
示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```



``` js
解法一：递归分治。
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
//分治
var maxDepth = function(root) {
    if(!root) {
        return 0;
    }
    let leftDepth = maxDepth(root.left);
    let rightDepth = maxDepth(root.right);
    return Math.max(leftDepth, rightDepth) + 1;
};

解法二：层序遍历BFS队列。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

var maxDepth = function (root) {
  if (!root) return 0;
  let maxRes = 0;
  const queue = [];
  queue.push(root);
  while (queue.length) {
    const length = queue.length;
    maxRes++;
    for (let i = 0; i < length; i++) {
      const node = queue.shift();
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return maxRes;
};
```





### [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

``` mathematica
示例：

输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1
备注:
这个问题是受到 Max Howell 的 原问题 启发的 ：

谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。


```



``` js
给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。

解法一：从根部翻转，再到内部递归翻转。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function (root) {
  if (!root) return root;

  [root.left, root.right] = [root.right, root.left];
   // 内部的翻转交给递归去做
  invertTree(root.left);
  invertTree(root.right);
  return root
};

解法二：BFS层序遍历。根节点先入列，然后出列，出列就 “做事”，交换它的左右子节点（左右子树）。
并且让左右子节点（左右子树）入列，往后，这些子节点（子树）会出列，也被翻转。
直到队列为空，就遍历完所有的节点（子树），翻转了所有子树，即翻转了整个二叉树。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function (root) {
  if (!root) return root;

  const queue = [];
  queue.push(root);
  while (queue.length) {
    const node = queue.shift();
    [node.left, node.right] = [node.right, node.left];
    node.left && queue.push(node.left);
    node.right && queue.push(node.right);
  }
  return root
};
```







### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

``` mathematica
示例 :
给定二叉树

          1
         / \
        2   3
       / \     
      4   5    
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。


注意：两结点之间的路径长度是以它们之间边的数目表示。
```



``` js
解法一：类似求最大深度的解法一，在递归的时候拿每个节点左右子树深度的和去比较最大值即可。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

var diameterOfBinaryTree = function(root) {
    let result = 0;

    const maxDepth = root => {
        if(!root) {
            return 0;
        }
        const left = maxDepth(root.left);
        const right = maxDepth(root.right);

        result = Math.max(result, left + right);
        return Math.max(left, right) + 1;
    }

    maxDepth(root);
    return result;
};

解法二：思路有点类似求最大深度的解法二。但有个注意点，就是仅仅是左边的也可以有最大直径。或者右边。比如上图。主要是模拟题意
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

function rootDeepLength(queue) {
  let length = 0;
  while (queue.length) {
    let queueTempLength = queue.length
    length++;
    for (let i = 0; i < queueTempLength; i++) {
      let node = queue.shift();
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return length;
}
var diameterOfBinaryTree = function (root) {
  if (!root) return 0;
  const leftQueue = [];
  const rightQueue = [];
  root.left && leftQueue.push(root.left);
  root.right && rightQueue.push(root.right);
  //分别计算以根节点为起点，根节点的左右子树的最大深度
  let left = rootDeepLength(leftQueue);
  let right = rootDeepLength(rightQueue);

  
  let rootLeft =diameterOfBinaryTree(root.left);//计算以根节点的左子树为起点的最大直径。
  let rootRight = diameterOfBinaryTree(root.right);//计算以根节点的右子树为起点的最大直径。
  return Math.max(left + right, rootLeft, rootRight);
};
```





### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

``` mathematica
示例 1:

输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
注意: 合并必须从两个树的根节点开始。
```



``` js
解法一:递归。如果把mergeTrees函数作为递归函数，它接收的t1和t2是指：当前遍历的节点（子树）
递归总是关注当前节点：

t1、t2 都存在，将 t2 的节点值加到 t1 上来。
t1 为 null、t2 不是 null，t1 换成 t2。
t2 为 null、t1 不是 null，t1 依然 t1。
t1 和 t2 都为 null，t1 依然 t1。

/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} t1
 * @param {TreeNode} t2
 * @return {TreeNode}
 */

var mergeTrees = function (t1, t2) {
  if (!t1 && t2) {
    return t2;
  }
  if (t1 && !t2 || (!t1&&!t2)) {
    return t1;
  }
  
  t1.val += t2.val;

  t1.left = mergeTrees(t1.left, t2.left);
  t1.right = mergeTrees(t1.right, t2.right);

  return t1;
};
```





## 链表





### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

![](D:\A-前端学习\图片\题目\21.jpg)

``` js
解法一：
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    let arr=[]
    while(l1){
      arr.push(l1.val)
      l1=l1.next
    }
    while(l2){
      arr.push(l2.val)
      l2=l2.next
    }
    arr.sort((v1,v2)=>v1-v2)
    if(arr.length==0) return l1	//这点得注意
    let arrNewList=arr.map(item=>{
      return new ListNode(item)
    })
    for(let i=0;i<arrNewList.length-1;i++){
      arrNewList[i].next=arrNewList[i+1]
    }
    return arrNewList[0]
};


解法二：使用递归实现，新链表也不需要构造新节点，我们下面列举递归三个要素
终止条件：两条链表分别名为 l1 和 l2，当 l1 为空或 l2 为空时结束
返回值：每一层调用都返回排序好的链表头
本级递归内容：如果 l1 的 val 值更小，则将 l1.next 与排序好的链表头相接，l2 同理
O(m+n)，m 为 l1的长度，n 为 l2 的长度
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function(list1, list2) {
    if(!list1) return list2;
    if(!list2) return list1;

    if(list1.val < list2.val) {
        list1.next = mergeTwoLists(list1.next,list2)
        return list1
    } else {
        list2.next = mergeTwoLists(list1,list2.next)
        return list2
    }
};


解法三：
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function(list1, list2) {
    const res = new ListNode()
    let node = res

    while(list1 && list2) {
        if(list1.val < list2.val) {
            node.next = list1
            node = node.next
            list1 = list1.next
        } else {
            node.next = list2
            node = node.next
            list2 = list2.next
        }
    }

    if(!list1) {
        node.next = list2
    } else {
        node.next = list1
    }
    return res.next
};
```





### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

给你一个链表的头节点 `head` ，判断链表中是否有环。

![](D:\A-前端学习\图片\题目\141.png)

``` js
解法一：给每个未访问的节点增加一个isVisited属性，访问过的即存在这个属性为true，如果下一个节点有这个属性，代表是个环
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    while(head!=null){
      if(head.isVisited){
        return true;
      }
      head.isVisited=true;
      head=head.next;
    }
    return false;
};

解法二：快、慢指针，从头节点出发
慢指针每次走一步，快指针每次走两步，不断比较它们指向的节点的值
如果节点值相同，说明有环。如果不同，继续循环。
类似 “追及问题”
两个人在环形跑道上赛跑，同一个起点出发，一个跑得快一个跑得慢，在某一时刻，跑得快的必定会追上跑得慢的，只要是跑道是环形的，不是环形就肯定追不上。

var hasCycle = (head) => {
  let fast = head;
  let slow = head;
  while (fast && fast.next) {                        
    slow = slow.next;                 
    fast = fast.next.next;             
    if (slow == fast) return true;   
  }
  return false;                   
}

解法三：Set
var hasCycle = function(head) {
    const visited = new Set();
    while (head !== null) {
        if (visited.has(head)) {
            return head;
        }
        visited.add(head);
        head = head.next;
    }
    return null;
};

解法四：超出限制
var hasCycle = function(head) {
    if (!head || !head.next) return false;
    let count = 0;
    while (head) {  
        count ++
        head = head.next
        if (count > 10000) {
            return true
        }
        if (head === null) {
            return false
        }
    }
    return false;
};
```







### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

图示两个链表在节点 c1 开始相交：

![](D:\A-前端学习\图片\题目\160.png)

``` js
解法一：和环形列表类似。在A链表上每个节点加一个isVisited属性，之后遍历B链表的时候判断其是否有这个属性标识，有则代表是相交节点。
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {
  while (headA != null) {
    headA.isVisitedA = true;
    headA = headA.next;
  }
  while (headB != null) {
    if (headB.isVisitedA) {
      return headB;
    }
    headB = headB.next;
  }
  return null;
};

解法三：可以理解成两个人速度一致， 走过的路程一致。那么肯定会同一个时间点到达终点。如果到达终点的最后一段路两人都走的话，那么这段路上俩人肯定是肩并肩手牵手的。
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {
  let pA = headA, pB = headB;
  while (pA != pB) {
    pA = pA == null ? headB : pA.next;
    pB = pB == null ? headA : pB.next;
  }
  return pA;
};
```



### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

![](D:\A-前端学习\图片\题目\206.jpg)

``` js
解法一:双指针滑动迭代。
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
(pre开始为null,cur为1，pre随即为下面的a,b,c,d,e,f等整体部分，cur为1,2,3,4,5,null等节点)
cur pre
1->	null (a),
2-> 1->null (b),
3-> 2->1->null (c)
4-> 3->2->1->null(d)
5-> 4->3->2->1->null(e)
null 5->4->3->2->1->null(f)

var reverseList = function(head) {
  let pre = null, cur = head, nextCur;
  while (cur != null) {
    nextCur = cur.next;//暂存下一个指针
    cur.next = pre;
    pre = cur;
    cur = nextCur;
  }
  return pre;
};

解法二：递归。
var reverseList = function (head) {
 let reverse = (pre,cur)=>{
     if(cur==null) return pre;
     let next = cur.next;
     cur.next=pre;
     return reverse(cur,next);
 }
 return reverse(null,head);  // !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!注意这里要return
};
```





### [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

![](D:\A-前端学习\图片\题目\234.jpg)

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

``` js
解法一：双指针
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {
    const arr = []
    while(head) {
        arr.push(head.val)
        head = head.next
    }
    let n = arr.length
    for(let i = 0,j = n - 1; i < n; i++,j--) {
        if(i >= j) break
        if(arr[i] != arr[j]) return false 
    }
    return true
};

var isPalindrome = function(head) {
    if (!head.next) return true;
    const arr = []
    while(head) {
        arr.push(head.val)
        head = head.next
    }
    let i = 0
    let n = arr.length
    while(i < n/2) {
        if(arr[i] !== arr[n-i-1]) {
            return false
        }
        i++;
    }
    return true
};


var isPalindrome = function(head) {
    if (!head.next) return true;
    const arr = []
    while(head) {
        arr.push(head.val)
        head = head.next
    }
    let reverseArr = [];
    for (let i = 0; i < arr.length; i++) {
        reverseArr[i] = arr[i]
    }
    reverseArr.reverse()
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] !== reverseArr[i]) {
            return false
        }
    }
    return true
};



解法二：快慢指针。快慢指针，起初都指向表头，快指针一次走两步，慢指针一次走一步，遍历结束时：
偶数个，slow 正好指向中间两个结点的后一个。
奇数个，slow 正好指向中间结点。
用 pre 保存 slow 的前一个结点，通过pre,.next = null断成两个链表。
注：在这里无需判断是否为偶数个还是奇数个，若为12321，就分成，12和321两个链表，slow指向3，pre指向2，翻转后面一个，再依次比较即可，若为1221，则为12和21，slow指向2，pre也指向2。其余同。

/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {
  if (!head || !head.next) return true;
  let slow = head;
  let fast = head;
  let pre;
  while (fast && fast.next) {
    pre = slow;
    slow = slow.next;
    fast = fast.next.next;
  }

  //从pre节点这断开总链表，分成前后两个
  pre.next = null;
  //从slow节点开始翻转后面一半的链表
  let head2 = null;
  while (slow) {
    const curNext = slow.next;
    slow.next = head2;
    head2 = slow;
    slow = curNext;
  }

  while (head) {
    if (head.val != head2.val) {
      return false;
    }
    head = head.next;
    head2 = head2.next
  }
  return true;
};


var isPalindrome = function(head) {
    let slow = head
    let fast = head
    let pre = slow
    while(fast && fast.next) {
        pre = slow
        slow = slow.next
        fast = fast.next.next
    }
    pre.next = null

    let prve = null
    let cur = slow
    let curNext;
    while(cur) {
        curNext = cur.next
        cur.next = prve
        prve = cur
        cur = curNext
    }
    while(prve && head){
        if(prve.val != head.val) return false
        prve = prve.next
        head = head.next
    }
    return true
};
```







## 数组



### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

输入：nums = [3,3], target = 6
输出：[0,1]
```

``` js
解法一：双指针
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    for(let i = 0; i < nums.length; i++) {
        for(let j = 0; j < i; j++) {
            if(nums[i] + nums[j] === target) return [i,j]
        }
    }
};

解法二：map
var twoSum = function(nums, target) {
    let map = new Map()
    for(let i = 0; i < nums.length; i++) {
        if(map.has(target - nums[i])) return [map.get(target - nums[i]), i];
        map.set(nums[i], i)
    }
};
```





### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

``` mathematica
示例:

输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

``` js
解法一：动态规划。
/**
 * @param {number[]} nums
 * @return {number}
 */[6,-3,-5,-2,4,5]
var maxSubArray = function(nums) {
  let sum=0,maxsum=-Infinity;
  for(let item of nums){
    sum=Math.max(sum+item,item)
      
    maxsum=Math.max(sum,maxsum)//如果有最大值就更新
  }
  return maxsum
};

解法二：动态规划。
状态:dp[i] 为以nums[i]结尾的最大子序和。则dp[i-1]为以nums[i-1]结尾的最大子序和。

转移方程：dp[i] = Math.max(dp[i - 1] + nums[i], nums[i])

初始条件：无

结果：Math.max(dp[0],dp[1],....,dp[n-1])

/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    const n = nums.length;
    const dp = [];
    for (let i = 0; i < n; i++) {
        dp[i] = nums[i];
        if (i > 0) {
            dp[i] = Math.max(dp[i - 1] + nums[i], dp[i])
        }
    }
    return Math.max(...dp);
};

var maxSubArray = function(nums) {
    const n = nums.length;
    const dp = [];
    dp[0] = nums[0]
    for (let i = 1; i < n; i++) {
        dp[i] = Math.max(dp[i - 1] + nums[i], nums[i])
    }
    return Math.max(...dp);
};
```





## 字符串



### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

``` mathematica
示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true
```

``` js
解法一：符号匹配，用栈处理就行
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
  let stack1=[];
  if(s==='') return true
  let signMap=new Map()
  signMap.set('(',')')
  signMap.set('[',']')
  signMap.set('{','}')
  for(let i=0;i<s.length;i++){
    if(s[i]==='('||s[i]==='['||s[i]==='{') stack1.push(s[i]);
    if(s[i]===')'||s[i]===']'||s[i]==='}') {
      if(signMap.get(stack1.pop())===s[i]) continue
      else return false
    }
  }
  if(stack1.length===0) return true		//注意出来的时候栈为空才行，如果栈不为空说明还有符号没匹配
  else return false
};


/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    const arr = []
    for(let i = 0; i < s.length; i++) {
        if(s[i] === '(' || s[i] === '{' || s[i] === '[') arr.push(s[i])
        if(s[i] === ')') {
            let last = arr.pop()
            if(last != '(') return false
        } 
        if(s[i] === '}') {
            let last = arr.pop()
            if(last != '{') return false
        } 
        if(s[i] === ']') {
            let last = arr.pop()
            if(last != '[') return false
        } 
    }
    if(arr.length) return false
    return true
};
```





# 思想

## 递归

每个都是一样的，思考一层就可以了,自己不用想他到底是怎么实现的，只要知道返回什么就行了。

if(!root) 是后来的叶子节点用的

最重要的是return什么 
