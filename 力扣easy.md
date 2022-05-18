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



#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

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

``` js
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

``` js
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



# 思想

## 递归

每个都是一样的，思考一层就可以了

if(!root) 是后来的叶子节点用的
