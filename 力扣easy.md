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



### 94.二叉树的中序遍历

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



# 思想

## 递归

每个都是一样的，思考一层就可以了
