

# LeetCode刷题记录(2020/07/20)

## 顺序(参考：算法小白如何高效、快速刷leetcode？ - lucifer的回答 - 知乎 https://www.zhihu.com/question/321738058/answer/1279464192)

### 基础篇(基础永远是最重要的，先把最最基础的这些搞熟，磨刀不误砍柴工)

- 数组，队列，栈
- 链表
- 树与递归
- 哈希表12
- 双指针

### 思想篇(这些思想是投资回报率极高的，强烈推荐每一个小的专题花一定的时间掌握)

- 二分
- 滑动窗口
- 搜索（BFS，DFS，回溯）
- 动态规划

### 提高篇(这部分收益没那么明显，并且往往需要一定的技术积累。出现的频率相对而言比较低。但是有的题目需要你使用这些技巧。又或者可以使用这些技巧可以实现「降维打击」。)

- 贪心
- 分治
- 位运算
- KMP & RK
- 并查集
- 前缀树
- 线段树
- 堆









## 数组

### 1343.大小为K且平均值大于等于阈值的子数组数目(注意子数组是连续的，子序列可以非连续)

给你一个整数数组 `arr` 和两个整数 `k` 和 `threshold` 。

请你返回长度为 `k` 且平均值大于等于 `threshold` 的子数组数目。

```mathematica
示例 1：

输入：arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
输出：3
解释：子数组 [2,5,5],[5,5,5] 和 [5,5,8] 的平均值分别为 4，5 和 6 。其他长度为 3 的子数组的平均值都小于 4 （threshold 的值)。

示例 2：

输入：arr = [1,1,1,1,1], k = 1, threshold = 0
输出：5

示例 3：

输入：arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
输出：6
解释：前 6 个长度为 3 的子数组平均值都大于 5 。注意平均值不是整数。

示例 4：

输入：arr = [7,7,7,7,7,7,7], k = 7, threshold = 7
输出：1

示例 5：

输入：arr = [4,4,4,4], k = 4, threshold = 1
输出：1
 

提示：

1 <= arr.length <= 10^5
1 <= arr[i] <= 10^4
1 <= k <= arr.length
0 <= threshold <= 10^4

```



```javascript
解法一：滑动窗口解法。这里的优化就是滑动窗口解法，拿当前和减去数组的第一个值，加上当前值，就滑动到了下一个子数组的总和，用总和比较方便点
var numOfSubarrays = function (arr, k, threshold) {
  let count = 0
  let targetSum=k*threshold
  let sum=0
  for(let i=0;i<k;i++) sum+=arr[i];
    
  if(sum>=targetSum) count++;
  for(let i=k;i<arr.length;i++){
    sum-=arr[i-k]
    sum+=arr[i]
    if(sum>=targetSum) count++
  }
  return count
};


```



### 面试题16.04. 井字游戏

设计一个算法，判断玩家是否赢了井字游戏。输入是一个 N x N 的数组棋盘，由字符" "，"X"和"O"组成，其中字符" "代表一个空位。

以下是井字游戏的规则：

玩家轮流将字符放入空位（" "）中。
第一个玩家总是放字符"O"，且第二个玩家总是放字符"X"。
"X"和"O"只允许放置在空位中，不允许对已放有字符的位置进行填充。
当有N个相同（且非空）的字符填充任何行、列或对角线时，游戏结束，对应该字符的玩家获胜。
当所有位置非空时，也算为游戏结束。
如果游戏结束，玩家不允许再放置字符。
如果游戏存在获胜者，就返回该游戏的获胜者使用的字符（"X"或"O"）；如果游戏以平局结束，则返回 "Draw"；如果仍会有行动（游戏未结束），则返回 "Pending"。



```mathematica
示例
输入： board = ["O X"," XO","X O"]
输出： "X"
```

```javascript
解法一：主要思想就是找出行、列、左右对角线的4种情况，一一突破	
var tictactoe = function (board) {
  let N = board.length
  let arr = board.join('').split('');

  for (let i = 0; i < arr.length - N + 1; i++) {
    if (arr[i] !== ' ' && isOK(i, arr, N)) { //空格不应该加到判断里去
      return arr[i]
    }
  }
  if (arr.includes(' ')) {
    return 'Pending'
  } else {
    return 'Draw'
  }
};
function isOK(index, arr, N) {
  let tempN = N
  let tempIndex = index

  if (index % N == 0) { //行
    while (--N) {
      if (arr[index] == arr[index + 1]) {
        index += 1;
      } else {
        break
      }
    }
    if (N == 0) return true
    N = tempN
    index = tempIndex
  }

  if (index >= 0 && index <= N - 1) { //列
    while (--N) {
      if (arr[index] == arr[index + tempN]) {
        index += tempN;
      } else {
        break
      }
    }
    if (N == 0) return true
    N = tempN
    index = tempIndex
  }

  if (index == 0) {//左上角斜对线
    while (--N) {
      if (arr[index] == arr[index + tempN + 1]) {
        index += tempN + 1;
      } else {
        break
      }
    }
    if (N == 0) return true
  }

  if (index == N - 1) { //右上角斜对线
    while (--N) {
      if (arr[index] == arr[index + tempN - 1]) {
        index += tempN - 1;
      } else {
        break
      }
    }
    if (N == 0) return true
  }

  return false
}


```

### 1. 两数之和（重做：）

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

```mathematica
示例
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

```

```javascript
解法一：主要思想：利用i,j双指针遍历判断即可，因为每种输入只会对应一个答案，即没有重复的元素
var twoSum = function (nums, target) {
  let arr=[]
  for (let i = 0;i<nums.length;i++){
    for(let j = i+1;j<nums.length;j++){
      if(nums[i]+nums[j]===target)
      arr.push(i,j)
    }
  }
  return arr
};


解法二：此题使用ES6的Map，使得时间复杂度降为O(n)
var twoSum = function(nums, target) {
    var map=new Map();
    for(let i=0;i<nums.length;i++){
        if(!map.has(target-nums[i])){ //元素值就是map的索引，下标是map的值，利用每种输入只会对应一个答案，即没有重复的元素
            map.set(nums[i],i);
        }else{
            return [map.get(target-nums[i]),i]
        }
    }
};
```



### 15. 三数之和（重做：）

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

```mathematica
示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

```js
解法一：分治。采用分治的思想找出三个数相加等于 0，我们可以数组依次遍历，每一项 a[i]我们都认为它是最终能够用组成 0 中的一个数字，那么我们的目标就是找到剩下的元素（除 a[i]）两个相加等于-a[i].

通过上面的思路，我们的问题转化为了给定一个数组，找出其中两个相加等于给定值，我们成功将问题转换为了另外一道力扣的简单题目1. 两数之和。这个问题是比较简单的， 我们只需要对数组进行排序，然后双指针解决即可。 加上需要外层遍历依次数组，因此总的时间复杂度应该是 O(N^2)。

/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function (nums) {
    if (nums.length < 3) return [];
    nums.sort((v1, v2) => v1 - v2);
    let res = [];

    for (let i = 0; i < nums.length; i++) {
        //nums is sorted,so it's impossible to have a sum = 0
        if (nums[i] > 0) break;
        
        // skip duplicated result without set
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        
        let left = i + 1, right = nums.length - 1;
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                res.push([nums[i], nums[left], nums[right]]);
                //如果下一个与当前左边这个相同，则右移
                while (left < right && nums[left] === nums[left + 1]) {
                    left++;
                }
                while (left < right && nums[right] === nums[right - 1]) {
                    right--;
                }
                left++,right--;//就算不相同也得更新左边和右边的数
            } else if (sum > 0) {
                right--;
            } else {
                left++;
            }
        }
    }
    return res;
};
```



### 4. 寻找两个正序数组的中位数(重做：*)（二分）

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

请你找出这两个正序数组的中位数，并且**要求算法的时间复杂度为 O(log(m + n))。**

你可以假设 nums1 和 nums2 不会同时为空。



```mathematica
示例
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

```javascript
解法一：二分。
m+n偶数时，i + j = m - i  + n - j ,也就是 j = ( m + n ) / 2 - i => 等效于m + n + 1) / 2 - i。
中位数 = （左半部分最大值 + 右半部分最小值 ）/ 2。
m+n奇数时，i + j = m - i  + n - j  + 1,也就是 j = ( m + n + 1) / 2 - i
中位数 = 左半部分最大值。 也就是左半部比右半部分多出的那一个数。
详见：https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/ 的解法4
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */

var findMedianSortedArrays = function (nums1, nums2) {
    // make sure to do binary search for shorten array
    // 保证 m <= n
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1]
    }
    const m = nums1.length
    const n = nums2.length
    let low = 0
    let high = m
    while (low <= high) {
        // const i = low + Math.floor((high - low) / 2)
        // const j = Math.floor((m + n + 1) / 2) - i
        const i = low + ((high - low) >> 1)
        const j = ((m + n + 1) >> 1) - i;
        const maxLeftA = i === 0 ? -Infinity : nums1[i - 1] //为了不越界，要保证 i != 0
        const minRightA = i === m ? Infinity : nums1[i]     //为了不越界，要保证 i != m
        const maxLeftB = j === 0 ? -Infinity : nums2[j - 1] //为了不越界，要保证 j != 0
        const minRightB = j === n ? Infinity : nums2[j]     //为了不越界，要保证 j != n   
        //左半部分最大的值小于等于右半部分最小的值 max ( A [ i - 1 ] , B [ j - 1 ]）） <= min ( A [ i ] , B [ j ]））
        //为了保证 max ( A [ i - 1 ] , B [ j - 1 ]）） <= min ( A [ i ] , B [ j ]）），因为 A 数组和 B 数组是有序的，所以 A [ i - 1 ] <= A [ i ]，B [ i - 1 ] <= B [ i ] 这是天然的，所以我们只需要保证 B [ j - 1 ] < = A [ i ] 和 A [ i - 1 ] <= B [ j ]

        if (maxLeftA <= minRightB && minRightA >= maxLeftB) {
            return (m + n) % 2 === 1
                ? Math.max(maxLeftA, maxLeftB)
                : (Math.max(maxLeftA, maxLeftB) + Math.min(minRightA, minRightB)) / 2
        } else if (maxLeftA > minRightB) { // i 需要减小
            high = i - 1
        } else {// maxLeftB > minRightA    //i需要增大
            low = low + 1
        }
    }
};

解法二：这里要注意的是，当数组长度为奇数时，不能直接/2，奇数/2会得到分数，需要用到Math.floor，除此之外数组的sort方法，默认只对正数排序，如果是负数的话会出错，所以得养成传排序函数的习惯
var findMedianSortedArrays = function (nums1, nums2) {
  nums1.push(...nums2)
  nums1.sort((v1,v2)=>v1-v2)
  if (nums1.length % 2 == 0) {
    return (nums1[nums1.length / 2 - 1] + nums1[nums1.length / 2]) / 2
  } else {
    return nums1[Math.floor(nums1.length / 2)]
  }
};
```



### 34. 在排序数组中查找元素的第一个和最后一个位置(重做：)（二分）

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

```mathematica
示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

```javascript
解法一：二分。
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

//排除法，详见：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/si-lu-hen-jian-dan-xi-jie-fei-mo-gui-de-er-fen-cha/ 、 https://leetcode-cn.com/leetbook/read/learning-algorithms-with-leetcode/xs41qg/
var searchRange = function (nums, target) {
    const len = nums.length;

    const findFirst = () => {
        if (!len) {
            return -1;
        }

        let left = 0, right = len - 1;
        while (left < right) {
            let mid = left + ((right - left) >> 1);
            if (nums[mid] < target) {// 小于一定不是开始位置解
                left = mid + 1; //[mid+1,right]
            } else if (nums[mid] >= target) {
                right = mid;        //[left,mid]
            }
        }

        if (nums[left] === target) {
            return left;
        }

        return -1;
    }

    const findLast = () => {
        if (!len) {
            return -1;
        }

        let left = 0, right = len - 1;
        while (left < right) {
            let mid = left + ((right - left + 1) >> 1);
            if (nums[mid] > target) { // 大于一定不是开始位置解
                right = mid - 1;  //[left,mid-1]
            } else if (nums[mid] <= target) {
                left = mid;       //[mid,right]   当搜索区间里只剩下两个元素的时候，一定要将取中间数的行为改成上取整，也就是在括号里加 1。不然会死循环，详见：https://leetcode-cn.com/leetbook/read/learning-algorithms-with-leetcode/xs41qg/
            }
        }

        if (nums[left] === target) {
            return left;
        }
        return -1;
    }

    return [findFirst(), findLast()];
};


解法二：有JS的api可以用indexOf，lastIndexOf
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
  let arr=[]
  const index1=nums.indexOf(target)
  arr.push(index1)
  const index2=nums.lastIndexOf(target)
  arr.push(index2)
  return arr
};
```



### 48. 旋转图像（重做：）

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

```mathematica
示例 1:
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

```

```javascript
解法一：先转置，然后再将每一行逆置就解决了
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],
[
  [1,4,7],
  [2,5,8],
  [3,6,9]
], 
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
var rotate = function (matrix) {
  for (let i = 0; i < matrix.length; i++) {
    for (let j = i; j < matrix.length; j++) { //这里是从i开始
      [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]]
    }
  }
  matrix=matrix.map(item => {
    return item.reverse()
  })
  return matrix
};
```



### 49. 字母异位词分组（重做：）

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

```mathematica
示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
说明：

所有输入均为小写字母。
不考虑答案输出的顺序。
```

```javascript
解法一：这些字母异位词通过排序后都会是一样的，就通过这个判断。先拿到一个去重的deWeightArr数组，这个数组里包含了所有的不同种类字符串，利用这个长度可以构造出一个确定数量的二维数组。再通过map将其中的字符串作为索引，下标作为值。最后再遍历push进对应的二维数组里面
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
var groupAnagrams = function(strs) {
  let arr=[]
  let strscopy=JSON.parse(JSON.stringify(strs))
  strscopy.forEach((item,index,arr)=>{
    arr[index]=item.split('').sort().join('')
  })
  const deWeightArr=[...new Set(strscopy)]
  let n=deWeightArr.length;
  let mymap=new Map()
  for(let i=0;i<n;i++){
    mymap.set(deWeightArr[i],i)
    arr.push([])
  }
  for(let i=0;i<strs.length;i++){
    let item=strs[i].split('').sort().join('')
    arr[mymap.get(item)].push(strs[i])
  }
  return arr
};

解法二：用26个字母数组去存。
var groupAnagrams = function(strs) {
    const map = new Map();

    for(let str of strs) {
        const keyArr = new Array(26).fill(0);
        for(let char of str) {
            keyArr[char.charCodeAt() - 97]++;
        }   
        // const key = keyArr.join(''); 这种不行，因为出现 0 1 0 10    0 10 1 0 的情况
        const key = keyArr.join(',');
        if(!map.has(key)) {
            map.set(key, []);
        }
        map.get(key).push(str);
    }
    
    return [...map.values()];
};
```



### 56. 合并区间（重做）

给出一个区间的集合，请合并所有重叠的区间。

```mathematica
示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:

输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

```javascript
解法一：//右边区间的左边界>左边区间的右边界时，不需要合并。//右边区间的左边界<=左边区间的右边界时，可以考虑合并，前提条件是仅在右边区间的右边界>左边区间的右边界时，才合并,如[1,3],[2,3]不合并,[1,3],[2,6]合并
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    if(intervals.length==0) return []
    intervals.sort((v1,v2)=>v1[0]-v2[0]) //将区间数组按左边界升序排列
    let arr=[]  //作为结果数组
    arr.push(intervals[0])
    for(let i=1;i<intervals.length;i++){
      if(intervals[i][0]>arr[arr.length-1][1]){//右边区间的左边界>左边区间的右边界时，不需要合并
        arr.push(intervals[i])
      }else{                                 //右边区间的左边界<=左边区间的右边界时，可以考虑合并
        if(intervals[i][1]>arr[arr.length-1][1]){ //仅在右边区间的右边界>左边区间的右边界时，才合													   并,如[1,3],[2,3]不合并,[1,3],[2,6]合并
          arr[arr.length-1][1]=intervals[i][1]
        }
      }
    }
    return arr
};

```



### 75. 颜色分类（重做：）

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

```mathematica
示例:

输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```


进阶：

一个直观的解决方案是使用计数排序的两趟扫描算法。
首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
你能想出一个仅使用常数空间的一趟扫描算法吗？



```javascript
解法一：利用JS特性，从两头添加0和2,1默认就跳过在中间。时间复杂度O（n^2）
var sortColors = function(nums) {
  let i=0,count=0;
  while(count<nums.length){
    if(nums[i]===0){
      nums.splice(i,1);
      nums.unshift(0);
      i++;            //i++才会让i到刚才那个位置的后面一个
    }
    if(nums[i]===2){
      nums.splice(i,1);
      nums.push(2);   //不需要i++，因为本身splice后就到下一个坐标了
    }
    if(nums[i]===1){
      i++;            //遇到1往右移
    }
    count++;
  }
};

解法二：双指针，交换数。
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
// p2 右边都是 2，p0 左边都是 0
var sortColors = function(nums) {
    let p0 = 0, p2 = nums.length - 1, cur = 0;
    while(cur <= p2) {
        if(nums[cur] === 2) {
            [nums[p2], nums[cur]] = [nums[cur], nums[p2]];
            p2--;//此时的cur位对应的值已经改变，不能cur++
        }else if(nums[cur] === 0) {
            [nums[p0], nums[cur]] = [nums[cur], nums[p0]];
            p0++, cur++;  //cur和p0都得向右
        }else{
            cur++;
        }
    }
};
```



### 136. 只出现一次的数字（重做：）

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```mathematica
示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4
```

```javascript
解法一：哈希，因为题目说了其余的均只出现两次，可以用对象先存着每个，再碰到的时候是相同的话直接删掉，则最后只剩下那出现一次的值了。
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
  let numsObj = {};
  for (let i = 0; i < nums.length; i++) {
    if (numsObj[nums[i]]) delete numsObj[nums[i]];
    else numsObj[nums[i]] = 1;
  }
  return Object.keys(numsObj)[0];
};

解法二：异或，这个方法一般想不到，对位运算的规律比较熟练才行。主要因为异或运算有以下几个特点：
一个数和 0 做 XOR 运算等于本身：a⊕0 = a
一个数和其本身做 XOR 运算等于 0：a⊕a = 0
XOR 运算满足交换律和结合律：a⊕b⊕a = (a⊕a)⊕b = 0⊕b = b

/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    let ans = 0;
    for(const num of nums) {
        ans ^= num;
    }
    return ans;
};

或者
var singleNumber = function(nums) {
    return nums.reduce((a,b)=> a^b)
};
```



### 448. 找到所有数组中消失的数字（重做：）

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在**不使用额外空间且时间复杂度为O(n)的情况下**完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

```mathematica
示例:

输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```

```javascript
解法一：在本身数组上面存信息。根据已有的值，去标记对应 index 值为负数
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var findDisappearedNumbers = function(nums) {
    // 根据已有的值，去标记对应 index 值为负数
    nums.forEach((num, index) => {
        const absNum = Math.abs(num)
        nums[absNum - 1] = -(Math.abs(nums[absNum - 1]));
    })
    //nums这时候为[-4, -3, -2, -7, 8,  2, -3, -1]，说明index为4,5的没有对应的值指向，即，5,6不存在

    // 查找哪些值不为负数，即没有数指向这个 index
    return nums.reduce((pre, num, index) => {
        num > 0 && pre.push(index + 1);
        return pre
    }, [])
};
```



### 739. 每日温度（重做：）

请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。



```javascript
解法一：单调栈。用一个栈stack来存温度数组的下标，没有遇到温度高的就先存到栈里，之后遇到高温度了再一个一个pop出来做处理。动画详见：https://leetcode-cn.com/problems/daily-temperatures/solution/cheng-xu-yuan-de-zi-wo-xiu-yang-739-daily-temperat/
/**
 * @param {number[]} T
 * @return {number[]}
 */

var dailyTemperatures = function(T) {
    const len = T.length;
    const stack = [], res = new Array(len).fill(0);
    for(let i = 0; i < len; ++i) {
        while(stack.length && T[stack[stack.length - 1]] < T[i]) {
            const lowerIndex = stack.pop();
            res[lowerIndex] = i - lowerIndex;
        }
        stack.push(i);
    }
    return res;
};

解法二：配合数组的findIndex函数
/**
 * @param {number[]} T
 * @return {number[]}
 */
var dailyTemperatures = function (T) {
  let result = []
  T.forEach((item, index) => {
    let retIndex = T.findIndex((item2, index2) => {
      return index2 > index && item2 > item
    });
    if (retIndex === -1) {
      retIndex = 0
    } else {
      retIndex = retIndex - index
    }
    result.push(retIndex)
  })
  return result
};
```



### 31. 下一个排列（重做：）

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

```javascript
解法一：思路就是默认右边是降序序列，找到第一个破坏降序序列的nums[i]，从降序序列中从右边起`第一个大于nums[i]的`，并将其和nums[i]进行交换。最后再将后面的降序序列reverse一下，就是最小的值了
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
function reverseRange(arr, i, j) {
  while (i < j) {
    [arr[i], arr[j]] = [arr[j], arr[i]];
    i++;
    j--;
  }
}
var nextPermutation = function (nums) {

  let i = nums.length - 2;
  // 从后往前找到第一个降序的,相当于找到了我们的回溯点3
 // 1 3 4 2     ->    1 4 2 3
  while (i > -1 && nums[i] >= nums[i+1]) i--;

  if (i > -1) {
    let j = nums.length - 1;
    // 找到从右边起第一个大于nums[i]的，并将其和nums[i]进行交换，找到了4
    // 因为如果交换的数字比nums[i]还要小肯定不符合题意
    while (nums[j] <= nums[i]) j--;
    [nums[j],nums[i]]=[nums[i],nums[j]];
  }
  reverseRange(nums,i+1,nums.length-1)
  return nums;
};
```



### 283. 移动零（重做：）

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺

```mathematica
示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。
```

```javascript
解法一：双指针
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function (nums) {
  let index = 0;
  //这个循环将数组里面全部去掉0了。
  for (let i = 0; i < nums.length; i++) {
    if (nums[i]!= 0) {
      nums[index++] = nums[i];
    }
  }
  for (let i = index; i < nums.length; i++){
    nums[i] = 0;
  }
  return nums;
};

解法二：双指针交换。
/** 
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    const len = nums.length;
    let lastNonZeroFound = 0;

    for(let i = 0; i < len; ++i) {
        if(nums[i]) {
            [nums[i], nums[lastNonZeroFound++]] = [nums[lastNonZeroFound], nums[i]]
        }
    }

};

解法三：直接用数组的splice和push方法，不过时间复杂度会变大。这里还得注意splice处理后，下标就不要右移了，i保持不变在下一轮。
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function (nums) {
  let length = nums.length;
  for (let i = 0; i < length; i++){
    if (nums[i] === 0) {
      nums.splice(i, 1);
      nums.push(0);
      length--; //不需要再遍历到最后了
      i--;//有splice的操作，i这里不要再++到下一个下标去
    }
  }
  return nums;
};
```



### 121. 买卖股票的最佳时机（重做：）

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票

```mathematica
示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```

```javascript
思考一下z
解法一：维持一个最小价格minNum，循环prices数组，有小于minNum的值则替换，否则更新计算最大利润。
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
  let minNum = prices[0];
  let res = 0;
  for (let i = 1; i < prices.length; i++) {
      minNum = Math.min(minNum,prices[i]);
      res = Math.max(res,prices[i] - minNum);
  }
  return res;
};

解法二：动态规划。时间复杂度高，为O(n^2),没有上面方法仅保持一个最小值快。 c

状态：第j天买，第i天卖,遍历最大值

转移方程：dp[i] = Math.max(dp[i], prices[i] - prices[j]); （prices[i] > prices[j]）

初始条件：无

结果Math.max(dp[0]，dp[1],....dp[n-1]);

/**
 * @param {number[]} prices
 * @return {number}
 */

// 当前到队首
// 不要从当前到末尾 // 超时
var maxProfit = function (prices) {
    const n = prices.length;
    if (n == 0) {
        return 0;
    }

    const dp = [];
    for (let i = 0; i < n; i++) {
        dp[i] = 0;
        for (let j = 0; j <=i; j++) {
            if (prices[i] > prices[j]) {
                dp[i] = Math.max(dp[i], prices[i] - prices[j]);
            }
        }
    }
    return Math.max(...dp)
};
```



### 169. 多数元素（数量大于一半的众数）（重做：）

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

```mathematica
示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2
```

```javascript
解法一：投票算法。
相同的加1, 不相同的减1, 因为是大于一半, 所以最后肯定剩下大于一半的那个
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
    let candidate = 0
    let count = 0
    for (let num of nums) {
        if (count === 0) candidate = num;
        count += (num === candidate ? 1 : -1)
    }
    return candidate
};

解法二：哈希表，用对象实现。
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
  const obj = {};
  for (let i = 0; i < nums.length; i++) {
    if (obj[nums[i]] != void 0) {
      obj[nums[i]] += 1;
    } else {
      obj[nums[i]] = 0;
    }
  }
  let max = -1;
  let maxIndex ;
  for (let i of Object.keys(obj)) {
    if (max < obj[i]) {
      max = obj[i];
      maxIndex = i;
    }
  }
  return maxIndex;
};

解法三：排序。如果将数组 nums 中的所有元素按照单调递增或单调递减的顺序排序，那么下标为floor后n/2的元素（下标从 0 开始）一定是众数。时间复杂度变高
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
  nums.sort((v1,v2)=>v1-v2);
  return nums[Math.floor(nums.length / 2)];
};

```



### 238. 除自身以外数组的乘积（重做：）

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

```mathematica
示例:

输入: [1,2,3,4]
输出: [24,12,8,6]
 

提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

```

```javascript
解法一：
两次遍历，一次正向，一次反向。
经过正向遍历，output中i项中存放的是前i-1项的乘积（不包含i项本身）
经过反向遍历，output中i项中符合题意了，相当于在每项中再乘以i项之后的乘积
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var productExceptSelf = function(nums) {
  let output = new Array(nums.length);
  //经过这次正向遍历，output中i项中存放的是前i项的乘积（不包含i项本身）
  for (let i = 0,temp=1; i < nums.length; i++){
    output[i] = temp;
    temp *= nums[i];
  }

  //经过这次反向遍历，output中i项中符合题意了，相当于在每项中再乘以i项之后的乘积
  for (let i = nums.length-1, temp = 1; i >=0; i--){
    output[i] *= temp;
    temp *= nums[i];
  }

  return output;
};
```



### 240. 搜索二维矩阵 II（重做：）

 编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

```javas
示例:
 
现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

```

![image-20201026152254600](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201026152254600.png)

```javascript
解法一:暴力法。
略
解法二：找规律。大了就往左走，小了就往右走。
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function(matrix, target) {
  if (matrix.length === 0) return false;

  let [col, row] = [matrix[0].length - 1, 0];
  while (col >= 0 && row < matrix.length) {
    if (matrix[row][col] > target) {
      col--;
    } else if(matrix[row][col] < target){
      row++;
    } else {
      return true;
    }
  }
  return false;
};
```



### 581. 最短无序连续子数组(重做：)

给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是**最短**的，请输出它的长度。

```mathematica
示例 1:

输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
说明 :

输入的数组长度范围在 [1, 10,000]。
输入的数组可能包含重复元素 ，所以升序的意思是<=。

```

```javascript
解法一：用一个排好序的副数组去比对即可。注意sort函数得传参，不然10,15的这种两位数会排序错误放到前面。
/**
 * @param {number[]} nums
 * @return {number}
 */
var findUnsortedSubarray = function(nums) {
  let tempNums = [...nums];
  tempNums.sort((v1, v2) =>v1-v2);
  let start = 1, end = 0; //如果初始就是升序，那么end-start+1 =0;
  let startFlag = false, endFlag = false;
  for (let i = 0,j=tempNums.length-1; i < tempNums.length && j>=0; i++,j--){
    if (tempNums[i] != nums[i]&&!startFlag) {
      start = i;
      startFlag = true;
    }
    if (tempNums[j] != nums[j] && !endFlag) {
      end = j;
      endFlag = true;
    }
  }
  return end - start + 1;
};

解法二：O(n)的时间复杂度。
```



### 128. 最长连续序列（重做：）

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

 **进阶：**你可以设计并实现时间复杂度为 `O(n)` 的解决方案吗？

```mathematica
示例 1：

输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
示例 2：

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
 

提示：

0 <= nums.length <= 104
-109 <= nums[i] <= 109
通过次数85,669提交次数164,

```



```javascript
解法一：时间复杂度O(n)的方法。
 数字连续序列，从最小的元素开始记录
 利用 numSet.has(num - 1) 使得 O(n)复杂度
 
/**
 * @param {number[]} nums
 * @return {number}
 */
var longestConsecutive = function(nums) {
    const numSet = new Set(nums);
    let result = 0, curLen;
    for(let num of numSet) {
        if(numSet.has(num - 1)) {
            continue;
        }
        curLen = 1;
        while(numSet.has(num + curLen)) {
            curLen++;
        }
        result = Math.max(result, curLen);
    }
    return result;
};

解法二：排序+去重，然后循环一遍取递增最大数。(nlogn)

/**
 * @param {number[]} nums
 * @return {number}
 */
var longestConsecutive = function (nums) {

    nums.sort((v1, v2) => v1 - v2);
    nums = [...new Set(nums)];//去重
    const n = nums.length;
    if (n == 0) {
        return 0;
    }
    if (n == 1) {
        return 1;
    }
    let maxLen = 1;
    let tempLen = 1;
    for (let i = 0; i < nums.length - 1; i++) {
        if (nums[i + 1] - nums[i] == 1) {
            tempLen++;
            maxLen = Math.max(tempLen, maxLen);
        } else {
            tempLen = 1;
        }
    }
    return maxLen;
};


```



### 152. 乘积最大子数组(重做：*)

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

```mathematica
示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

```

```javascript
解法一：双重循环遍历，把所有的情况考虑到（暴力)，每次取最大值，这里需要注意的是得在判断第二重循环前加最大值比较，因为有[0,2]的这种情况

/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function (nums) {
    const n = nums.length;
    if (n == 1) {
        return nums[0];
    }
    let ret;
    let maxnum = -Infinity;
    for (let i = 0; i < n; i++) {
        ret = nums[i];
        maxnum = Math.max(maxnum, ret);//有[0,2]的这种情况
        for (let j = i+1; j < n; j++) {
            ret *= nums[j];
            maxnum = Math.max(maxnum, ret);
        }
    }
    return maxnum;
};

解法二：动态规划。
/**
 * @param {number[]} nums
 * @return {number}
 */
// dpMin 表示以第 i 个元素结尾的乘积最小子数组的乘积
// 因为正负性问题，当前元素的最优解不简单依赖于上一个元素的最优解
// 如果当前为正数，依赖于上一个元素结尾的最大子数组成绩单
// 如果当前为负数，依赖于上一个元素结尾的最小子数组乘积
var maxProduct = function(nums) {
    const dpMax = [nums[0]], dpMin = [nums[0]];
    for(let i = 1; i < nums.length; ++i) {
        dpMax[i] = Math.max(nums[i] * dpMax[i-1], nums[i] * dpMin[i-1], nums[i]);
        dpMin[i] = Math.min(nums[i] * dpMax[i-1], nums[i] * dpMin[i-1], nums[i]);
    }
    return Math.max(...dpMax);
};
```

### 54. 螺旋矩阵（重做：1，*）

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

```mathematica
示例 1:

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

```

![image-20201217224153981](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201217224153981.png)

```js
解法一：每一层不全部遍历完，留一个。（上右下左顺时针）
如果一条边从头遍历到底，则下一条边遍历的起点随之变化
选择不遍历到底，可以减小横向、竖向遍历之间的影响
一轮迭代结束时，4条边的范围，同时收窄 1
循环做的工作更清晰：遍历一个“圈” & 范围收缩至内圈
top bottom left right分别表示四条边
var spiralOrder = function (matrix) {
  if (matrix.length === 0) return []
  const res = []
  let top = 0, bottom = matrix.length - 1, left = 0, right = matrix[0].length - 1
  while (top < bottom && left < right) {
    for (let i = left; i < right; i++) res.push(matrix[top][i])   // 上层
    for (let i = top; i < bottom; i++) res.push(matrix[i][right]) // 右层
    for (let i = right; i > left; i--) res.push(matrix[bottom][i])// 下层
    for (let i = bottom; i > top; i--) res.push(matrix[i][left])  // 左层
    right--
    top++
    bottom--
    left++  // 四个边界同时收缩，进入内层
  }
  if (top === bottom) // 剩下一行，从左到右依次添加
    for (let i = left; i <= right; i++) res.push(matrix[top][i])
  else if (left === right) // 剩下一列，从上到下依次添加
    for (let i = top; i <= bottom; i++) res.push(matrix[i][left])
  return res
};


```



### 88. 合并两个有序数组（重做：）（双指针）

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

说明：

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

```mathematica
示例：

输入：
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出：[1,2,2,3,5,6]
 

提示：

-10^9 <= nums1[i], nums2[i] <= 10^9
nums1.length == m + n
nums2.length == n

```

```js
解法一：双指针。从后往前。时间O(m+n),空间O(1)
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function (nums1, m, nums2, n) {
    let index = m + n - 1;
    m-- , n--;
    //主要是把nums2里面的数合并到nums，所以循环判断n即可，m就算没遍历完也是一个有序的状态
    while (n >= 0) {
        if (m >= 0 && nums1[m] > nums2[n]) {
            nums1[index--] = nums1[m--];
        } else {
            nums1[index--] = nums2[n--];
        }
    }

};
```



## 滑动窗口

### **滑动窗口算法的思路**

```mathematica
1、我们在字符串 S 中使用双指针中的左右指针技巧，初始化 left = right = 0，把索引左闭右开区间 [left, right) 称为一个「窗口」。

2、我们先不断地增加 right 指针扩大窗口 [left, right)，直到窗口中的字符串符合要求（包含了 T 中的所有字符）。

3、此时，我们停止增加 right，转而不断增加 left 指针缩小窗口 [left, right)，直到窗口中的字符串不再符合要求（不包含 T 中的所有字符了）。同时，每次增加 left，我们都要更新一轮结果。

4、重复第 2 和第 3 步，直到 right 到达字符串 S 的尽头。

这个思路其实也不难，**第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」，最终找到最优解，**也就是最短的覆盖子串。左右指针轮流前进，窗口大小增增减减，窗口不断向右滑动，这就是「滑动窗口」这个名字的来历。

下面画图理解一下，needs 和 window 相当于计数器，分别记录 T 中字符出现次数和「窗口」中的相应字符的出现次数。

```

初始状态：

![image-20201029152102370](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201029152102370.png)

增加 right，直到窗口 [left, right) 包含了 T 中所有字符：

![image-20201029152110985](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201029152110985.png)

现在开始增加 left，缩小窗口 [left, right)。

![image-20201029152116034](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201029152116034.png)

直到窗口中的字符串不再符合要求，left 不再继续移动。

![image-20201029152123193](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201029152123193.png)

```javascript
/* 滑动窗口算法框架 */

var findAnagrams = function(s, p) {
  let need = {};//存储子串各字符的个数
  let window = {};//存放窗口
  for (let char of p) {
    need[char] ? need[char]++ : need[char]=1;
  }

  const needLength = Object.keys(need).length;
  let left = 0, right = 0;
  let valid = 0;//记录
 
  while (right< s.length) {
    let char = s[right];
    right++;//像右滑动
    
    //进行窗口内的一系列更新
   ....
   
    //判断左侧窗口是否要收缩
    while (window needs shrink) {
	  // d 是将移出窗口的字符
      let d = s[left];
      left++;
	  // 进行窗口内数据的一系列更新
	  ....
    }
  }
 
};


```

**只需要思考以下四个问题**：

1、当移动 right 扩大窗口，即加入字符时，应该更新哪些数据？

2、什么条件下，窗口应该暂停扩大，开始移动 left 缩小窗口？

3、当移动 left 缩小窗口，即移出字符时，应该更新哪些数据？

4、我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

如果一个字符进入窗口，应该增加 window 计数器；如果一个字符将移出窗口的时候，应该减少 window 计数器；当 valid 满足 need 时应该收缩窗口；应该在收缩窗口的时候更新最终结果。

### 438. 找到字符串中所有字母异位词（重做：）

给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

说明：

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。

```mathematica
示例 1:

输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
 示例 2:

输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。

```

```javascript
解法一：滑动窗口。
/**
 * @param {string} s
 * @param {string} p
 * @return {number[]}
 */
var findAnagrams = function(s, p) {
  let need = {};//存储子串各字符的个数
  let window = {};//存放窗口
  for (let char of p) {
    need[char] ? need[char]++ : need[char]=1;
  }

  const needLength = Object.keys(need).length;
  let left = 0, right = 0;
  let valid = 0;//记录
  let res = [];
  while (right< s.length) {
    let char = s[right];
    right++;//像右滑动
    //进行窗口内的一系列更新
    if (need[char]) {
      window[char] ? window[char]++ : window[char]=1;
      if (window[char] === need[char]) {
        valid++;
      }
    }
    //判断左侧窗口是否要收缩
    if (right-left ==p.length) {
      if (valid === needLength) {
        res.push(left);
      }
      // d 是将移出窗口的字符
      let d = s[left];
      left++;
      // 进行窗口内数据的一系列更新
      if (need[d]) {
        if (need[d] === window[d]) {
          valid--;
        }
        window[d]--;
      }
    }
  }
  return res;
};
```



### 76. 最小覆盖子串（重做：）

给你一个字符串 S、一个字符串 T 。请你设计一种算法，可以在 O(n) 的时间复杂度内，从字符串 S 里面找出：包含 T 所有字符的最小子串。

```mathematica
示例：

输入：S = "ADOBECODEBANC", T = "ABC"
输出："BANC"
 

提示：

如果 S 中不存这样的子串，则返回空字符串 ""。
如果 S 中存在这样的子串，我们保证它是唯一的答案。
```

```javascript
解法一：滑动窗口。
/**
 * @param {string} s
 * @param {string} p
 * @return {number[]}
 */
var minWindow = function(s, t) {
  let need = {};//存储子串各字符的个数
  let window = {};//存放窗口
  for (let char of t) {
    need[char] ? need[char]++ : need[char]=1;
  }

  const needLength = Object.keys(need).length;
  let left = 0, right = 0;
  let valid = 0;//记录
  let start = 0, len=Infinity;
  while (right< s.length) {
    let char = s[right];
    right++;//像右滑动
    //进行窗口内的一系列更新
    if (need[char]) {
      window[char] ? window[char]++ : window[char]=1;
      if (window[char] === need[char]) {
        valid++;
      }
    }
    //判断左侧窗口是否要收缩
    while (valid === needLength) {
      if (right - left < len) {
        len = right - left;
        start = left;
      }
      // d 是将移出窗口的字符
      let d = s[left];
      left++;
      // 进行窗口内数据的一系列更新
      if (need[d]) {
        if (need[d] === window[d]) {
          valid--;
        }
        window[d]--;
      }
    }
  }
  return len === Infinity ? "" : s.slice(start, start+len);
};
```



## 二分法

### 287. 寻找重复数（重做：*）

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。（只有一个重复数）

**仔细看算法的书写过程**

```mathematica
示例 1:
输入: [1,3,4,2,2]
输出: 2

示例 2:
输入: [3,1,3,4,2]
输出: 3
说明：

不能更改原数组（假设数组是只读的）。
只能使用额外的 O(1) 的空间。
时间复杂度小于 O(n2) 。
数组中只有一个重复的数字，但它可能不止重复出现一次。

```

```javascript
解法一：双指针。二分法的思路是先猜一个数（有效范围 [left, right]里的中间数 mid），然后统计原始数组中小于等于这个中间数的元素的个数 cnt，如果 cnt 严格大于 mid，（注意我加了着重号的部分「小于等于」、「严格大于」）。根据抽屉原理，重复元素就在区间 [left, mid] 里；

以 [2, 4, 5, 2, 3, 1, 6, 7] 为例，一共 8 个数，n + 1 = 8，n = 7，根据题目意思，每个数都在 1 和 7 之间。

例如：区间 [1, 7][1,7] 的中位数是 4，遍历整个数组，统计小于等于 4 的整数的个数，如果不存在重复元素，最多为 4 个。等于 4 的时候区间 [1, 4]内也可能有重复元素。但是，如果整个数组里小于等于 4 的整数的个数严格大于 4 的时候，就可以说明重复的数存在于区间 [1, 4]。


/**
 * @param {number[]} nums
 * @return {number}
 */
var findDuplicate = function(nums) {
  let left = 1;
  let right = nums.length - 1;
  while (left < right) {
    let mid = (left+right)>>1;
    let count = 0;

    for (let num of nums) {
      if (num <= mid) {
        count++; //如果整个数组里小于等于 mid 的整数的个数严格大于 mid 的时候，就可以说明重复的数存在于区间 [left, mid]
      }
    }

    if (count > mid) {
       // 重复的元素一定出现在 [left, mid] 区间里
      right = mid;
    } else {
       // [mid + 1, right]
      left = mid + 1;
    }
  }
  //循环结束条件即left=right，区间只剩下一个元素，即重复值
  return left;
};

解法二：快慢指针。类似142找入环的第一个节点。
var findDuplicate = function(nums) {
    let slow = nums[0], fast = nums[nums[0]];
    while(slow !== fast) {
        slow = nums[slow];
        fast = nums[nums[fast]];
    }
    slow = 0;
    while(slow !== fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
};
```



### 33. 搜索旋转排序数组（重做：*）

升序排列的整数数组 nums 在预先未知的某个点上进行了旋转（例如， [0,1,2,4,5,6,7] 经旋转后可能变为 [4,5,6,7,0,1,2] ）。

请你在数组中搜索 target ，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

**注意判断条件**

```mathematica
示例 1：

输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
示例 2：

输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
示例 3：

输入：nums = [1], target = 0
输出：-1
 

提示：

1 <= nums.length <= 5000
-10^4 <= nums[i] <= 10^4
nums 中的每个值都 独一无二
nums 肯定会在某个点上旋转
-10^4 <= target <= 10^4

```

```js
解法一：二分，旋转后的数组是相对意义上的部分有序数组。
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    if(!nums.length){
        return -1
    }
    let length = nums.length
    let left = 0
    let right = length - 1
    //二分法总有一方有序
    while(left <= right){
        let mid = (left+right)>>1
        if(nums[mid] == target){
            return mid
        }
        // 判断nums[mid]是处于左段还是右段
        if(nums[mid] >= nums[left]){// 处于左段
            
            // 如果mid是处于左段段，那么从left 到 mid 是递增的。
            // 判断target是否处于这个区间内，如果是 则 right= mid-1， 否则 left = mid+1
            if(nums[mid] > target && target >= nums[left]){
                right = mid - 1
            }else{
                left = mid + 1
            }
        }
        else{// 处于右段
            // 如果mid是处于右段段，则 从mid 到right 这个区间是递增段，
            // 判断target是否在这个区间之内，如果是 则 left = mid + 1， 否则high=mid-1
            if(target <= nums[right] && nums[mid] < target){
                left = mid + 1
            }else{//若target位于右边无序区域
                right = mid  - 1
            }
        }
    }
    return -1
};
```





## 链表

### 2. 两数相加（重做：）

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```mathematica
示例：
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

```javascript
解法一:用一个变量存同一位相加得到的总和，再去根据条件赋值，这里的一个巧妙之处是用了||来判断了位数不同的情况
	  还有点要注意的是要返回头结点才行，所以得设置个头结点指针
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    const res = new ListNode(0);
    let ptr = res;
    let adv = 0;
    while(l1 || l2) {
        ptr.next = l1 || l2;
        ptr = ptr.next;
        if(l1) {
            adv += l1.val;
            l1 = l1.next;
        }
        if(l2) {
            adv += l2.val;
            l2 = l2.next;
        }
        ptr.val = adv % 10;
        adv = adv > 9? 1: 0;
    }
    if(adv !== 0) {
        ptr.next = new ListNode(adv);
    }
    return res.next;
};

```



### 19. 删除链表的倒数第N个节点（重做：）

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

```mathematica
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

```javascript
解法一：快慢指针。 
 一次遍历，两个指针
 需要找到要删除的节点的前一个节点
 first 先走 n+1 次（带头结点），然后 first 和 second 同时走，直到 first 为 null

/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */

//假设10个，删除倒数第2，先让快的走到第3个，再开始一起走，等fast走到10时，slow在7位置，fast走到null,slow在8，slow.next=slow.next.next 即可
var removeNthFromEnd = function(head, n) {
    let length = 0, step = 0;

    const dummyNode = new ListNode(0);
    dummyNode.next = head;

    let firstNode = dummyNode, secondNode = dummyNode;

    while(n >= 0) {
        firstNode = firstNode.next;
        n--;
    }//走到第n+1个了

    while(firstNode) {
        firstNode = firstNode.next;
        secondNode = secondNode.next;
    }

    secondNode.next = secondNode.next.next;

    return dummyNode.next;
};

解法二：受两数相加解法一的思路，将链表的操作可以转换成数组的操作，再通过数组的map重新new出每个节点放到新数组里，再循环一遍将next联系起来就成为了的新的链表。在这里有个需要注意的点就是，当只有一个数时且被删除了，这时候应该返回空链表，之前我返回new ListNode()时是不对的，返回[]更加不对，因为类型就不是链表。最后想到head指向遍历完后，指向的空，返回了head，果然就对了。
**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let arr=[]
    while(head){
      arr.push(head.val)
      head=head.next
    }
    arr.splice(-n,1)
    if(arr.length==0)return head	//注意这个的处理
    let newArrList=arr.map((item)=>{
      return new ListNode(item)
    })
    for(let i=0;i<newArrList.length-1;i++){
      newArrList[i].next=newArrList[i+1]
    }
    return newArrList[0]
};

```



### 21. 合并两个有序链表（重做：）

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

```mathematica
示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



```javascript
解法一：方法同第4题寻找两个正序数组的中位数，讲链表操作转换成数组操作就行，注意的就是为空链表时，返回空链表就行
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
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    if(l1 === null){
        return l2;
    }
    if(l2 === null){
        return l1;
    }
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else{
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};


解法三：循环遍历比较大小
var mergeTwoLists = function(l1, l2)  {
    //dummy 假的 虚的
  const dummyHead = {};
  let current = dummyHead;
  // l1: 1 -> 3 -> 5
  // l2: 2 -> 4 -> 6
  while (l1 !== null && l2 !== null) {
    if (l1.val < l2.val) {
      current.next = l1; // 把小的添加到结果链表
      current = current.next; // 移动结果链表的指针
      l1 = l1.next; // 移动小的那个链表的指针
    } else {
      current.next = l2;
      current = current.next;
      l2 = l2.next;
    }
  }

  if (l1 === null) {
    current.next = l2;
  } else {
    current.next = l1;
  }
  return dummyHead.next;
}
```



### 23. 合并K个升序链表（重做：）

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```mathematica
示例 1：

输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
示例 2：

输入：lists = []
输出：[]
示例 3：

输入：lists = [[]]
输出：[]
 

提示：

k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] 按 升序 排列
lists[i].length 的总和不超过 10^4

```

```js
解法一：类似归并排序。合并 K 个排序链表 与 合并两个有序链表 的区别点在于操作有序链表的数量上，因此完全可以按照上面的代码思路来实现合并 K 个排序链表。

这里可以参考 归并排序 的分治思想，将这 K 个链表先划分为两个 K/2 个链表，处理它们的合并，然后不停的往下划分，直到划分成只有一个或两个链表的任务，开始合并。
const mid = lists.length >> 1;//这句的效果妙，3>>1会等于1，不需要再用Math.floor了

function mergeTwoLists(l1, l2) {
    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l2.next, l1);
        return l2;
    }
};

/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function (lists) {
    if (lists.length == 0) return null;
    if (lists.length == 1) return lists[0];
    if (lists.length == 2) {
        return mergeTwoLists(lists[0], lists[1]);
    }

    const l1 = [], l2 = [];
    const mid = lists.length >> 1;//这句的效果妙，3>>1会等于1，不需要再用Math.floor了
    for (let i = 0; i < mid; i++) {
        l1.push(lists[i]);
    }
    for (let i = mid; i < lists.length; i++) {
        l2.push(lists[i]);
    }

    return mergeTwoLists(mergeKLists(l1), mergeKLists(l2));
};
```



### 141. 环形链表（重做：）

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 ```mathematica
示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
 ```

![image-20200908085742479](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200908085742479.png)

```mathematica
示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

![image-20200908085806395](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200908085806395.png)

```mathematica
示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

![image-20200908085836022](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200908085836022.png)

```javascript
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
```



### 142. 环形链表 II（重做：*）

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表

**进阶：**

- 你是否可以使用 `O(1)` 空间解决此题？

**示例 同上题：**

```javascript
解法一：解法同上题。给每个未访问的节点增加一个isVisited属性，访问过的即存在这个属性为true，如果下一个节点有这个属性，则代表进到了第一个环的节点。
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
var detectCycle = function(head) {
    while(head!=null){
      if(head.isVisited){
        return head;
      }
      head.isVisited=true;
      head=head.next;
    }
    return false;
};


解法二：Set
var detectCycle = function(head) {
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

解法三：快慢指针。
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
// 设链表中环外部分的长度为 a  slow 指针进入环后，又走了 b 的距离与 fast 相遇。此时，fast 指针已经走完了环的 n 圈，因此它走过的总距离为 a+n(b+c)+b=a+(n+1)b+nc
// 任意时刻，fast 指针走过的距离都为slow 指针的 2 倍。因此，我们有
// a+(n+1)b+nc=2(a+b)⟹a=c+(n−1)(b+c)
// 会发现：从相遇点到入环点的距离加上 n-1 圈的环长，恰好等于从链表头部到入环点的距离。
// 因此，当发现 slow 与fast 相遇时，我们将一个从起点；随后，他们每次向后移动一个位置。最终，它们会在入环点相遇。

// 链接：https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/

// 1. 双指针，两个快慢走，碰到时候，一个从起点，一个继续同步走，遇到的地方就是位置
var detectCycle = function(head) {
    // 边界情况 1->2
    if(!head || !head.next) {
        return null;
    }

    // 这里如果 fast = head.next , while(fast && fast.next)  会超时
    let slow = head, fast = head;
    while(fast && fast.next) {
        // 这里必须先走，再判断
        slow = slow.next;
        fast = fast.next.next;
        if(slow === fast) {
            break;
        } 
    }

    if(slow !== fast) {
        return null;
    }
    slow = head;
    
    while(slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
};
```



### 160. 相交链表（重做：*）

编写一个程序，找到两个单链表相交的起始节点。

![image-20201020200422620](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201020200422620.png) 

![image-20201020200438690](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201020200438690.png)

```mathematica
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

![image-20201020200515033](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201020200515033.png)

```mathematica
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。

```

![image-20201020200553058](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201020200553058.png)

```mathematica
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。

```

注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

```javascript
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

解法二：利用set结构来存储A链表里面的所有节点。每次遍历B时判断set中是否有也行。和上面思路相似。
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
  const mySet = new Set();
  while (headA != null) {
    mySet.add(headA);
    headA = headA.next;
  }
  while (headB != null) {
    if (mySet.has(headB)) {
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



### 206. 反转链表（重做：）

反转一个单链表。

**示例:**

```mathematica
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

```javascript
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
 return reverse(null,head);
};
```

### 92. 反转链表 II（重做：*）

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

```mathematica
示例:

输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

```js
解法一：构造头结点，划分区间反转。
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    //头节点
    let preHead = new ListNode(0);
    preHead.next = head;
    let pos = 0;
    let tmpHead = preHead;
    while(pos < m - 1){
        tmpHead = tmpHead.next;
        pos++;
    }//此时tmpHead指向第m-1个节点
    let pre = null;
    let cur = tmpHead.next;
    while(pos < n){
        let next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
        pos++;
    }
    //修改m和n-m位置处的节点的指向
    tmpHead.next.next = cur;
    tmpHead.next = pre;
    return preHead.next;
};

```



### 25. K 个一组翻转链表（重做：1）

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

 

```mathematica
示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

说明：

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。


```

```js
解法一：递归。容易理解！！！分成k组去反转，递归做这个事情
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    let pre = null, cur = head;
    let p = head;
    // 下面的循环用来检查后面的元素是否能组成一组
    for(let i = 0; i < k; i++) {
        if(p == null) return head;
        p = p.next;
    }
    for(let i = 0; i < k; i++){
        let next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
    }
    // pre为本组最后一个节点，cur为下一组的起点
    head.next = reverseKGroup(cur, k);
    return pre;
};

解法二：模拟。构造一个头结点。可见官网题解
const myReverse = (head, tail) => {
    let prev = tail.next;
    let cur = head;
    while (prev !== tail) {
        const nex = cur.next;
        cur.next = prev;
        prev = cur;
        cur = nex;
    }
    return [tail, head];
}
var reverseKGroup = function(head, k) {
    const preHead = new ListNode(0);
    preHead.next = head;
    let pre = preHead;

    while (head) {
        let tail = pre;
        // 查看剩余部分长度是否大于等于 k
        for (let i = 0; i < k; ++i) {
            tail = tail.next;
            if (!tail) {
                return preHead.next;
            }
        }
        const nex = tail.next;
        [head, tail] = myReverse(head, tail);
        // 把子链表重新接回原链表
        pre.next = head;
        tail.next = nex;
        pre = tail;
        head = tail.next;
    }
    return preHead.next;
};

```

### 24. 两两交换链表中的节点（重做：）

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

![image-20201218024610718](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201218024610718.png)

 ```mathematica
示例1：
输入：head = [1,2,3,4]
输出：[2,1,4,3]

示例 2：
输入：head = []
输出：[]

示例 3：
输入：head = [1]
输出：[1]
 

提示：

链表中节点的数目在范围 [0, 100] 内
0 <= Node.val <= 100

 ```

![image-20201219222559959](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201219222559959.png)

```js
解法一：递归。递归函数就是给一个节点，其能根据这个节点作为头的链表两两交换
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */

var swapPairs = function (head) {
    if (!head || !head.next) {
        return head;
    }
    let node1 = head, node2 = head.next;
    node1.next = swapPairs(node2.next);
    node2.next = node1;
    return node2;
};

解法二：利用25题k个一组，传2就行。
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */

var reverseKGroup = function(head, k) {
    let pre = null, cur = head;
    let p = head;
    // 下面的循环用来检查后面的元素是否能组成一组
    for(let i = 0; i < k; i++) {
        if(p == null) return head;
        p = p.next;
    }
    for(let i = 0; i < k; i++){
        let next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
    }
    // pre为本组最后一个节点，cur为下一组的起点
    head.next = reverseKGroup(cur, k);
    return pre;
};
var swapPairs = function(head) {
    return reverseKGroup(head,2);
};
```



### 234. 回文链表（重做：1）

请判断一个链表是否为回文链表。

```mathematica
示例 1:

输入: 1->2
输出: false
示例 2:
12321
输入: 1->2->2->1
输出: true
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？
```

```js
解法一：很容易想到转成数组。再用双指针从前后比较数组里面的是否为回文数组。时间复杂度O(n) ，空间复杂度O(n)。
比较简单，代码略。

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
```



### 143. 重排链表（*）

 给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```mathematica
示例 1:

给定链表 1->2->3->4, 重新排列为 1->4->2->3.
示例 2:

给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.

```

```js
解法一：模拟+双指针。
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null
 * }
 */
/**
 * @param {ListNode} head
 * @return {void} Do not return anything, modify head in-place instead.
 */
var reorderList = function(head) {
  if (!head) return;
    let list = [];
    let current = head;
    while (current) {
      list.push(current);
      current = current.next;
    }
    let i = 0, j = list.length - 1;
    while (i < j) {
      list[i].next = list[j];
      i ++;
      list[j].next = list[i];
      j --;
    }
    list[i].next = null
};

解法二：找中点，反转，断开，merge归并

var reorderList = function(head) {
    if(!head || !head.next) {
        return head;
    }
    
    let mid = findMiddle(head);
    let tail = reverse(mid.next);
    mid.next = null;
    
    merge(head, tail);
};

// 头确定，不需要dummy
function reverse(head) {
    let firstNode = null; // 始终指向开头第一个节点
    while(head) {
        let temp = head.next;
        head.next = firstNode;
        firstNode = head;
        head = temp;
    }
    return firstNode;
}
//一个一个间隔插进去
function merge(head1, head2) {
    let dummy = new ListNode(0);
    
    while(head1 && head2) {
        dummy.next = head1;
        head1 = head1.next;
        dummy = dummy.next

        dummy.next = head2;
        head2 = head2.next;
        dummy = dummy.next;
    }
    dummy.next = head1 ? head1 : head2;
}

function findMiddle(head) {
    let slow = head, fast = head;
    while(fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

```



## 哈希表、集合

### 560. 和为K的子数组（重做：1）

 给定一个整数数组和一个整数 **k，**你需要找到该数组中和为 **k** 的连续的子数组的个数。

```mathematica
示例 1 :

输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
说明 :

数组的长度为 [1, 20,000]。
数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。

```

![image-20201027222544352](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201027222544352.png)



![image-20201027222608686](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201027222608686.png)

```javascript
解法一：暴力遍历法。
const subarraySum = (nums, k) => {
  let count = 0;
  for (let i = 0; i < nums.length; i++) {
      let sum = 0;
    for (let j = i; j < nums.length; j++) {
        sum += nums[j];
      if (sum == k) count++;
    }
  }
  return count;
};

解法二：引入哈希表简化前缀和。
// [j,i] k === pre[i] - pre[j-1]
// 所以我们考虑以 i 结尾的和为 k 的连续子数组个数时只要统计有多少个前缀和为 pre[i]−k 的 pre[j-1] 即可
const subarraySum = (nums, k) => {
  let myMap = {0:1};
  let prefixSum  = 0;
  let count = 0;
  for (let i = 0; i < nums.length; i++){
    prefixSum += nums[i];
    
    if (myMap[prefixSum-k]) {
      count += myMap[prefixSum - k];
    }

    if (myMap[prefixSum]===void 0) {
      myMap[prefixSum] = 1;
    } else {
      myMap[prefixSum]++;
    }
  }
  return count;
};


```



### 41. 缺失的第一个正数（重做：1，*）

给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。

```mathematica
示例 1:

输入: [1,2,0]
输出: 3
示例 2:

输入: [3,4,-1,1]
输出: 2
示例 3:

输入: [7,8,9,11,12]
输出: 1

```

```js
解法一：将数组本身作为哈希。
答案肯定在[1,len+1]的index
1.把原数组作为 hash 表，打标记

2 转为字符串标记


/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    const len = nums.length;
    if(!len) {
        return 1;
    }
    
    for(let i = 0; i < len; ++i) {
        const num = +nums[i];
        (num > 0) && (nums[num] += '');
    }

    for(let i = 1; i <= nums.length; ++i) {
        if(typeof nums[i] !== 'string') {
            return i;
        }
    }
    return len;
};

```

### 剑指 Offer 03. 数组中重复的数字（重做：1，*）

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```mathematica
示例 1：
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
 

限制：

2 <= n <= 100000
```

```js
解法一：原地哈希。 一个萝卜一个坑的想法,
如果有重复元素，例如：

 nums[i]     1  2  3  2    萝卜
     i       0  1  2  3    坑
同样的，0号坑不要1号，先和1号坑交换，交换完这样的：

 nums[i]     2  1  3  2    萝卜
     i       0  1  2  3    坑
0号坑不要2号萝卜，去和2号坑交换，交换完这样的：

 nums[i]     3  1  2  2    萝卜
     i       0  1  2  3    坑
0号坑不要3号萝卜，去和3号坑交换，交换完这样的：

 nums[i]     2  1  2  3    萝卜
     i       0  1  2  3    坑
0号坑不要2号萝卜，去和2号坑交换，结果发现你2号坑也是2号萝卜，那我还换个锤子，同时也说明有重复元素出现。
详见：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/yuan-di-zhi-huan-shi-jian-kong-jian-100-by-derrick/

var findRepeatNumber = function (nums) {
    const length = nums.length;
    for (let i = 0; i < length; ++i) {
        //检测下标为i的元素是否放在了位置i上
        while ((num = nums[i]) !== i) {
            if (num === nums[num]) {
                return num;
            }
            [nums[i], nums[num]] = [nums[num], nums[i]];
        }
    }
};
```



## 树与递归

### 779. 第K个语法符号

在第一行我们写上一个 `0`。接下来的每一行，将前一行中的`0`替换为`01`，`1`替换为`10`。

给定行数 `N` 和序数 `K`，返回第 `N` 行中第 `K`个字符。（`K`从1开始）

```mathematica
例子:

输入: N = 1, K = 1
输出: 0

输入: N = 2, K = 1
输出: 0

输入: N = 2, K = 2
输出: 1

输入: N = 4, K = 5
输出: 1

解释:
第一行: 0
第二行: 01
第三行: 0110
第四行: 01101001

```

![image-20201016113323216](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201016113323216.png)

```javascript
解法一：递归。但是这题如果不列举一些情况出来，是不容易找到递归条件的，所以面对这类“找规律”题发现递归条件的，可以先在纸上画出来
据此可以总结出规律，第 K 个数字是上一行第 (K+1) / 2 个数字生成的。如果上一行的数字为 0，被生成的数字为 1 - (K%2)，如果上一行的数字为 1，被生成的数字为 K%2。
/**
 * @param {number} N
 * @param {number} K
 * @return {number}
 */
var kthGrammar = function (N, K) {
  if (N == 1) {
    return 0;
  }
  if (K % 2 == 1) {//奇数
    return kthGrammar(N - 1, (K + 1) / 2);
  } else {
    return kthGrammar(N - 1, K / 2) === 0 ? 1 : 0;
  }
};
```



### 938. 二叉搜索树的范围和（重做：1）

给定二叉搜索树的根结点 `root`，返回 `L` 和 `R`（含）之间的所有结点的值的和。

二叉搜索树保证具有唯一的值。

```mathematica
示例 1：

输入：root = [10,5,15,3,7,null,18], L = 7, R = 15
输出：32
示例 2：

输入：root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
输出：23


提示：

树中的结点数量最多为 10000 个。
最终的答案保证小于 2^31。

```

![image-20201016163434510](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201016163434510.png)

```javascript
解法一：递归，二叉搜索树，中序遍历后是一颗有序的值。当前节点 X < L 时，那么左子树肯定小于L，则返回右子树之和。当前节点 X > R 时，那么右子树肯定大于R，则返回左子树之和。当前节点 X >= L 且 X <= R 时则返回：当前节点值 + 左子树之和 + 右子树之和
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} L
 * @param {number} R
 * @return {number}
 */
var rangeSumBST = function (root, L, R) {
  if (!root) {
    return 0;
  }
  //当前节点 X < L 时，那么左子树肯定小于L，则返回右子树之和
  if (root.val < L) {
    return rangeSumBST(root.right, L, R);
  }
  //当前节点 X > R 时，那么右子树肯定大于R，则返回左子树之和
  if (root.val > R) {
    return rangeSumBST(root.left, L, R);
  }
  //当前节点 X >= L 且 X <= R 时则返回：当前节点值 + 左子树之和 + 右子树之和
  return root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R);
};
```



### 94. 二叉树的中序遍历(重做：1)

给定一个二叉树，返回它的中序 遍历。

```mathematica
示例:

输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```



```javascript
解法一：中序遍历，左->根->右
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

解法二：迭代遍历
const inorderTraversal = (root) => {
  const res = [];
  const stack = [];

  while (root) {        // 先把能压入栈的左子节点都压进来
    stack.push(root);
    root = root.left;
  }
  while (stack.length) {
    let root = stack.pop(); // 栈顶的节点出栈
    res.push(root.val);     // 在压入右子树之前，处理它的数值部分（中序遍历的定义）
    root = root.right;      // 获取它的右子树
    while (root) {          // 右子树存在，执行while循环    
      stack.push(root);     // 压入当前root
      root = root.left;     // 不断压入左子节点
    }
  }
  return res;
};

```

![image-20201031225712807](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201031225712807.png)



### 144.二叉树的前序遍历

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

```js
解法一：dfs递归
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
function dfs(res, root) {
  if (!root) return;
  res.push(root.val);
  dfs(res, root.left);
  dfs(res, root.right);
}
var preorderTraversal = function (root) {
    const res = [];
    dfs(res, root);
    return res;
};

解法二：迭代+栈
var preorderTraversal = function(root) {
    const res = [], stack = [];
    if(!root) {
      return res;
    }
    stack.push(root);
    while(stack.length) {
        const node = stack.pop();
        res.push(node.val);
        node.right && stack.push(node.right);
        node.left && stack.push(node.left);
    }
    return res;
};

```







### 96. 不同的二叉搜索树（重做：1）

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

```mathematica
示例:

输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3

```



```javascript
解法一：二叉搜索树，左子树<根节点<右子树。
假设n个节点存在二叉排序树的个数是G(n)，1为根节点，2为根节点，...，n为根节点，当1为根节点时，其左子树节点个数为0，右子树节点个数为n-1，同理当2为根节点时，其左子树节点个数为1，右子树节点为n-2，所以可得G(n) = G(0)*G(n-1)+G(1)*(n-2)+...+G(n-1)*G(0)
/**
 * @param {number} n
 * @return {number}
 */
var numTrees = function (n) {
  if (n <= 1) {
    return 1;
  }
  let res = 0;
  for (let i = 0; i < n; i++) {
    res += numTrees(i) * numTrees(n - i - 1);
  }
  return res;
};

解法二：动态规划
/**
 * @param {number} n
 * @return {number}
 */
// 以 m 为根节点，左子树形成的二叉搜索树个数 * 右子树形成的二叉搜索树个数
// dp[i] = 1 ~ i 之和 sum(dp[m-1]*dp[i-m])) 1 <= m <= i

var numTrees = function(n) {
    const dp = new Array(n+1).fill(0);
    dp[0] = 1, dp[1] = 1;
    for(let i = 2; i <= n; ++i) {
        for(let m = 1; m <= i; ++m) {
            dp[i] += dp[m-1] * dp[i-m];
        }
    }
    return dp[n];
};
```



### 98. 验证二叉搜索树（重做：1，*）

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

```mathematica
示例 1:

输入:
    2
   / \
  1   3
输出: true
示例 2:

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

```

```javascript
解法一：
1: 中序遍历是升序的
2：定义：也只需要左节点的值小于根节点,右节点的值小于根节点
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
 * @return {boolean}
 */
var isValidBST = function(root) {
    //验证一个树是否符合二叉搜索树
    function helper(root, lower, upper) {
        // 初始根节点为空或者一直递归检查到末端空节点，表明此时符合BST
        if (root == null) return true;
        if (root.val <= lower || root.val >= upper) return false;
        return helper(root.left, lower, root.val) && helper(root.right, root.val, upper);
    }
    return helper(root, -Infinity, Infinity);
};

解法二：中序遍历二叉搜索树。遍历得到的值放到数组里一定是个有序的数组。
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
function dfs(resArr,root) {
  if (!root) return;
  dfs(resArr,root.left);
  resArr.push(root.val);
  dfs(resArr, root.right);
}
var isValidBST = function(root) {
  const res = [];
  dfs(res, root);
  for (let i = 0; i < res.length-1; i++){
    if (res[i] >= res[i + 1]) return false;
  }
  return true;
};
```



### 101. 对称二叉树(重做：1）

给定一个二叉树，检查它是否是镜像对称的。

```mathematica
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
```

```javascript
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
var isSymmetric = function(root) {
  if (!root) return true; 
  const queue = [];
  queue.push(root.left, root.right);
  while (queue.length != 0) {
    const length = queue.length;//每一层的节点层序遍历
    for (let i = 0; i < length; i+=2) {
      const left = queue.shift();
      const right = queue.shift();
      if (left == null && right == null) {
        continue;
      }
      if ((left == null && right) || (right == null && left)) {
        return false;
      }
      if (left.val != right.val) {
        return false;
      }
      queue.push(left.left, right.right);
      queue.push(left.right, right.left);
    }
  }
  return true;
};

var isSymmetric = function(root) {
    if(root==null)return true;
    const queue=[];
    queue.push(root.left,root.right);
    while(queue.length){
        const len=queue.length;
        for(let i=0;i<len;i+=2){
            const left=queue.shift();
            const right=queue.shift();
            if((left&&right==null)||(left==null&&right)){
                return false;
            }
            if(left&&right){
                if(left.val!=right.val)return false;
                queue.push(left.left,right.right);
                queue.push(left.right,right.left);
            }      
        }
    }
    return true;
};

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



### 102. 二叉树的层序遍历(重做：1，2)

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

```mathematica
示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

```

```javascript
解法一：层序遍历队列BFS。每一层就是一个for循环。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
  if (!root) return [];
  const res = [];
  const queue = [];
  queue.push(root);
  while (queue.length) {
    const length = queue.length;
    const tempRes = [];
    //一次循环就是遍历每一层
    for (let i = 0; i < length; i++){
      const node = queue.shift();
      tempRes.push(node.val);
      node.left&&queue.push(node.left);
      node.right&&queue.push(node.right);
    }
    res.push(tempRes);
  }
  return res;
};
```

：

### 103. 二叉树的锯齿形层次遍历（重做：1）

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

```mathematica
例如：
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回锯齿形层次遍历如下：

[
  [3],
  [20,9],
  [15,7]
]

```

```js
解法一：和上题类似。只不过要判断层数01交替push和unshift进去每一层的值。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var zigzagLevelOrder = function (root) {
    if (!root) return [];
    const res = [];
    const queue = [];
    queue.push(root);
    let curLevelDepth = 0;

    while (queue.length) {
        const length = queue.length;
        const tempRes = [];
        //一次循环就是遍历每一层
        for (let i = 0; i < length; i++) {
            const node = queue.shift();
            curLevelDepth % 2 ? tempRes.unshift(node.val) : tempRes.push(node.val)
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        res.push(tempRes);
        curLevelDepth++;
    }
    return res;
};
```



### 104. 二叉树的最大深度（重做：1）

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

```mathematica
示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```

```javascript
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



### 199. 二叉树的右视图（重做：1）

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

```mathematica
示例:

输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---


```

```js
解法一：BFS，每次拿每层的最后那个就行了。
var rightSideView = function (root) {
    if (!root) return []
    const queue = [root]                        // 队列 把树顶加入队列
    let res = []                              // 用来存储每层最后个元素值
    while (queue.length > 0) {
        let len = queue.length
        while (len) {
            let node = queue.shift()               // 取出队列第一个元素
            if (len === 1) res.push(node.val)       // 当是 当前一层的最后一个元素时，把值加入res
            node.left && queue.push(node.left)    // 继续往队列添加元素
            node.right && queue.push(node.right)
            len--
        }
    }
    return res
};

```





### 剑指 Offer 55 - II. 平衡二叉树（重做：1）

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

```mathematica
示例 1:

给定二叉树 [3,9,20,null,null,15,7]

    3
   / \
  9  20
    /  \
   15   7
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。

 

限制：

1 <= 树的结点个数 <= 10000

```



```js
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
var isBalanced = function(root) {
    //判断一棵树是否符合平衡二叉树，不是就-1，是的话就返回深度
    function helper(node){
        if(node == null) return 0;
        let left = helper(node.left);
        if(left == -1) return -1;
        let right = helper(node.right);
        if(right == -1) return -1;
        return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
    }
    return helper(root) != -1;
};
```



### 543. 二叉树的直径（重做：1）

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

```mathematica
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

![image-20201029203058297](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201029203058297.png)

```javascript
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



### 105. 从前序与中序遍历序列构造二叉树（重做：1）

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

```mathematica
例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7

```

![image-20201018103553250](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201018103553250.png)



![image-20201018103617968](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201018103617968.png)

```javascript
解法一：递归，双指针。先序遍历的顺序是根节点，左子树，右子树。中序遍历的顺序是左子树，根节点，右子树。
只需要根据先序遍历得到根节点，然后在中序遍历中找到根节点的位置，它的左边就是左子树的节点，右边就是右子树的节点。生成左子树和右子树就可以递归的进行了。
注意下边的两个指针指向的数组范围是包括左边界，不包括右边界。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
//关键是把图都画出来，二叉树左子树的情况和右子树的情况
function buildNode(preorder, preStart, preEnd, inorder, inStart, inEnd) {
  // preorder 为空，直接返回 null
  if (preStart == preEnd) {
    return null;
  }
  const root = new TreeNode(preorder[preStart]);
  let rootIndexInorder;
  //在中序遍历中找到根节点的位置
  for (let i = inStart; i < inEnd; i++) {
    if (inorder[i] == preorder[preStart]) {
      rootIndexInorder = i;
      break;
    }
  }
  const leftNum = rootIndexInorder - inStart;
  //构造左子树
  root.left = buildNode(preorder, preStart + 1, preStart + 1 + leftNum, inorder, inStart, rootIndexInorder);
  //构造右子树
  root.right = buildNode(preorder, preStart + 1 + leftNum, preEnd, inorder, rootIndexInorder + 1, inEnd);
  return root;
}
var buildTree = function (preorder, inorder) {
  //两个指针指向的数组范围是包括左边界，不包括右边界。
  return buildNode(preorder, 0, preorder.length, inorder, 0, inorder.length);
};
```



### 114. 二叉树展开为链表（重做：1）

给定一个二叉树，原地将它展开为一个单链表。

 ```mathematica
例如，给定二叉树

    1
   / \
  2   5
 / \   \
3   4   6
将其展开为：

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6

 ```

```javascript
解法一：迭代
// 过程就是，找到左子树最右边的节点，把右子树接过去，左子树再移到右边
// 继续下一层，迭代
    1
   / \
  2   5
 / \   \
3   4   6

//将 1 的左子树插入到右子树的地方
    1
     \
      2         5
     / \         \
    3   4         6        
//将原来的右子树接到左子树的最右边节点
    1
     \
      2          
     / \          
    3   4  
         \
          5
           \
            6
            
 //将 2 的左子树插入到右子树的地方
    1
     \
      2          
       \          
        3       4  
                 \
                  5
                   \
                    6   
        
 //将原来的右子树接到左子树的最右边节点
    1
     \
      2          
       \          
        3      
         \
          4  
           \
            5
             \
              6         

var flatten = function(root) {
    while(root) {
        if(root.left) {
           let node = root.left;
           while(node.right) {
               node = node.right;
           } 
           node.right = root.right;
           root.right = root.left;
           root.left = null;
        }
        root = root.right;
    }
};
解法二：前序遍历（先序遍历），得到的值放到数组里，再将数组里的值接到root上。注意，root为空时，直接返回root即可。
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
 * @return {void} Do not return anything, modify root in-place instead.
 */
function dfs(res,root) {
  if (!root) return;
  res.push(root.val);
  dfs(res,root.left);
  dfs(res,root.right);
}
var flatten = function (root) {
  if (!root) return root;
  const res = [];
  dfs(res, root);
  root.val = res[0];
  for (let i = 1; i < res.length; i++){
    root.left = null;
    root.right = new TreeNode(res[i]);
    root = root.right;
  }
};
```



### 226. 翻转二叉树（重做：1）

翻转一棵二叉树。

```mathematica
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

![image-20201024173453131](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201024173453131.png)

```javascript
解法一：从根部翻转，再到内部递归翻转。
/*
 * @Author: 氢氟酸hefan
 * @Date: 2019-08-27 10:48:26
 * @LastEditTime: 2020-10-24 17:35:41
 * @LastEditors: 氢氟酸hefan
 * @Description: 
 * @FilePath: \yunniubaoc:\Users\缘分tbb&hf\Desktop\test\test.js
 */
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



### 236. 二叉树的最近公共祖先（重做：1）

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![image-20201026141805466](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201026141805466.png)

```mathematica
示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
 

说明:

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。
```

```javascript
解法一：p、q 的出现，有两种可能：
1.p、q 分居 root 节点的左右子树，则 LCA 为 root 节点。
2.p、q 同在 root 节点的一个子树中，则转为一个局部的子问题。
lowestCommonAncestor 函数：返回当前子树中 p 和 q 的 LCA。如果没有，就返回 null。

如果当前遍历的节点 root，不是 p 或 q 或 null，则我们要递归搜寻左右子树：
1.如果左右子树递归调用，都有结果，说明 p 和 q 分居 root 的左右子树，返回 root。
2.如果只是其中一个子树递归调用有结果，说明 p 和 q 都在这个子树，则返回该子树递归调用的结果。
3.如果两个子树递归调用的结果都为 null，说明 p 和 q 都不在这俩子树中，返回 null。

/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
//lowestCommonAncestor 函数：返回当前子树中 p 和 q 的 LCA最近公共祖先
var lowestCommonAncestor = function(root, p, q) {
  if (!root) return root;
  if (root === p || root === q) return root;

  const left = lowestCommonAncestor(root.left, p, q);
  const right = lowestCommonAncestor(root.right, p, q);
  //都有结果，说明 p 和 q 分居 root 的左右子树，返回 root。
  if (left && right) return root;
  //只是其中一个子树递归调用有结果，说明 p 和 q 都在这个子树，则返回该子树递归调用的结果。
  else return left ? left : right;
};
```



### 617. 合并二叉树（重做：1）

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

```mathematica
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

```javascript
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



### 判断两个二叉树是否完全一致（重做：1）

```js
interface TreeNode {
    value: number
    left: TreeNode || null
    right: TreeNode || null
}
diff (
{value: 1, left: {value: 2}, right: {value: 3},
{value: 1, left: {value: 2, left: {value: 1}, right: {value: 3}}}
)
```

```js
解法一：树+递归
function diff(TreeNode root1, TreeNode root2) {
	if (root1 == null && root2 == null) {
		return true;
	}
	if (root1 == null ||  root2 == null) {
		return false;
	}
	if (root1.val != root2.val) {//判断每个节点的值是否相等，如果去除此判断，则判断两个二叉树是否结构相等
		return false;
	}
	return diff(root1.left, root2.left) && diff(root1.right, root2.right);
}
```



### 538. 把二叉搜索树转换为累加树（重做：1）

给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。
注意：本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

![image-20201031225932342](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201031225932342.png)

```mathematica
示例 2：

输入：root = [0,null,1]
输出：[1,null,1]
示例 3：

输入：root = [1,0,2]
输出：[3,3,2]
示例 4：

输入：root = [3,2,4,1]
输出：[7,9,4,10]
 

提示：

树中的节点数介于 0 和 104 之间。
每个节点的值介于 -104 和 104 之间。
树中的所有值 互不相同 。
给定的树为二叉搜索树。


```

```javascript
解法一：递归，中序遍历的反向遍历。右根左
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * 
 * @param {TreeNode} root
 * @return {TreeNode}
 */ 

var convertBST = function (root) {
  let sum = 0;//给下面的闭包调用，值一直在
  function inOrderReverse(root) {
    if (!root) return;

    inOrderReverse(root.right);

    sum += root.val;//直接影响到外面的sum变量，闭包，外面的sum相当于是个convertBST内的“全局”变量;
    root.val = sum;

    inOrderReverse(root.left);
  }
  inOrderReverse(root);
  return root;
};

解法二：迭代。具体可见中序遍历的迭代方法，然后累加即可
const convertBST = (root) => {
  let sum = 0;
  let stack = [];
  let cur = root;

  while (cur) {  // 右子节点，不断压栈
    stack.push(cur);
    cur = cur.right;
  }

  while (stack.length) {  // 直到清空递归栈
    cur = stack.pop();    // 位于栈顶的节点出栈
    sum += cur.val;       // 做事情
    cur.val = sum;        // 做事情
    cur = cur.left;       // 找左子节点
    while (cur) {         // 存在，左子节点压栈
      stack.push(cur);    // 
      cur = cur.right;    // 找当前左子节点的右子节点，不断压栈
    }
  }

  return root;
};
```



### 124. 二叉树中的最大路径和（重做：1）

给定一个非空二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。**该路径至少包含一个节点，且不一定经过根节点。**

```mathematica
示例 1：

输入：[1,2,3]

       1
      / \
     2   3

输出：6
示例 2：

输入：[-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出：42

```

```javascript
解法一：递归。
递归函数意义：求出当前子树能向父节点“提供”的最大路径和。
一条从父节点延伸下来的路径，不能走了左子树，又掉头走右子树，不能两头收益。
如果一个子树提供的最大路径和为负，路径走入它，收益不增反减，该子树不该被考虑，让它返回 0，成为死支，像砍掉一样。
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
var maxPathSum = function (root) {
    let maxSum = -Infinity;

    // 返回包含当前节点,当前节点只用了一条边的最大路径和 (另一个用于连接父节点)
    const dfs = (root) => {
        if (!root) {
            return 0;
        }

        const left = dfs(root.left);//左子树的最大收益
        const right = dfs(root.right); //右子树的最大收益

        maxSum = Math.max(root.val + left + right, maxSum);
        const outputSum = root.val + Math.max(left, right);//一条从父节点延伸下来的路径，不能走了左子树，又掉头走右子树，不能两头收益。
        if (outputSum < 0) {
            return 0;
        } else {
            return outputSum;
        }
    }
    dfs(root);
    return maxSum;
};

```



### 337. 打家劫舍 III（重做：1）

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

```mathematica
示例 1:

输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
示例 2:

输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.

```

```javascript
解法一：递归。
递归函数就是求一颗树能盗取的最大金额。
打劫根节点，则不能打劫左右子节点。但是能打劫左右子节点的四个子树（如果有）。
不打劫根节点，则能打劫左子节点和右子节点
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
var rob = function (root) {
    let maxSum = 0;

    //递归函数表示一棵树能偷的最大金额
    const dfs = (root) => {
        if (!root) {
            return 0;
        }

        const left = dfs(root.left);
        const right = dfs(root.right);


        let tempSum = root.val;
        if (root.left) {
            const ll = dfs(root.left.left);
            const lr = dfs(root.left.right);
            tempSum += ll + lr;
        }
        if (root.right) {
            const rl = dfs(root.right.left);
            const rr = dfs(root.right.right);
            tempSum += rl + rr;
        }
        maxSum = Math.max(left + right, tempSum);
        return maxSum;
    }
    dfs(root);
    return maxSum;
};

解法二：记忆化递归。因为做了重复计算。我们计算了 root 的四个孙子子树，又计算了 root 的左右子树，而后者会把 root 的孙子子树重复计算一遍。
把计算过的结果存到map里缓存着，如果下次遇上了，有的话就直接取出来。速度会提升很多。

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
var rob = function (root) {
    let maxSum = 0;

    const memory = new Map();
    //递归函数表示一棵树能偷的最大金额
    const dfs = (root) => {
        if (!root) {
            return 0;
        }
        if (memory.has(root)) return memory.get(root);

        const left = dfs(root.left);
        const right = dfs(root.right);


        let tempSum = root.val;
        if (root.left) {
            const ll = dfs(root.left.left);
            const lr = dfs(root.left.right);
            tempSum += ll + lr;
        }
        if (root.right) {
            const rl = dfs(root.right.left);
            const rr = dfs(root.right.right);
            tempSum += rl + rr;
        }
        maxSum = Math.max(left + right, tempSum);
        memory.set(root, maxSum);
        return maxSum;
    }
    dfs(root);
    return maxSum;
};
```



## 字符串

### 3. 无重复字符的最长子串（重做：1）

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

```mathematica
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

```javascript
解法一：滑动窗口
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
    let l=r= 0, maxLen = 0;
    let myMap = new Map();
    while(r<s.length){
        let pos = myMap.get(s[r]);
        if(pos >=l && pos <=r){
            l= pos +1; //窗口缩小
        }
        myMap.set(s[r],r);
        maxLen=Math.max(maxLen,r-l+1);
        r++;			//窗口变大
    }
    return maxLen;
};


解法二:slice和indexOf方法巧妙解题
var lengthOfLongestSubstring = function(s) {
  let j=0,t=0,maxlen=0;
  for(let i=0;i<s.length;i++){
    t=s.slice(j,i).indexOf(s[i])  //s.slice(j,i)是当前符合条件的子串，s[i]是当前子串的右边一个字母，用indexOf判断其是否应该加进来
    if(t===-1){//当前子串长度先存着
      maxlen=Math.max(maxlen,i-j+1)
    }else{//当右边的一个字母不符合时，将找到重复字母下标的右边一个字母作为重新子串的第一个字母开始
      j=j+t+1     
    }
  }
  return maxlen
};

解法三：2019年暑假做的，有点复杂，基本上做完后隔了很久就不知道当时怎么做的了
ar lengthOfLongestSubstring = function(s) {
    let arrs=s.split('')
    let maxlen=0
    let len=0
    for(let i=0;i<s.length;i++){
        let ns=[]
        let j=i
        do{
            ns.push(s[j++])
            len=ns.length
            maxlen=Math.max(len,maxlen)
        }while(issame(s[j],ns)&&j<s.length)
    }
    return maxlen
};
function issame(s,ns){
    let flag=1
    ns.forEach((item)=>{
        if(item===s)
            flag=0
    })
    return flag
}
```



### 7.整数反转（重做：1）

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

```mathematica
示例
输入: 123
输出: 321

```

注意:

**假设我们的环境只能存储得下 32 位的有符号整数，**则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，**如果反转后整数溢出那么就返回 0。**

```javascript
解法一：
result * 10 + x % 10 取出末位 x % 10（负数结果还是负数，无需关心正负），拼接到 result 中。
x / 10 去除末位，| 0 强制转换为32位有符号整数。
通过 | 0 取整，无论正负，只移除小数点部分（正数向下取整，负数向上取整）。
result | 0 超过32位的整数转换结果不等于自身，可用作溢出判断。
运算过程:

x	result
123	0
12	3
1	32
0	321
代码

/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    let result = 0;
    while(x !== 0) {
        result = result * 10 + x % 10;
        x = (x / 10) | 0; 
    }
    return (result | 0) === result ? result : 0;
};

解法二：
var reverse = function(x) {
  if(x<0){
      x='-'+x.toString().slice(1).split('').reverse().join('')
  }else{
      x=x.toString().slice(0).split('').reverse().join('')
  }
  if(Number(x)>=2**31-1 || Number(x)<=(-2)**31){//指数表示:**
      return 0
  }
  return x
};
```



### 面试题 16.26. 计算器

给定一个包含正整数、加(+)、减(-)、乘(*)、除(/)的算数表达式(括号除外)，计算其结果。

表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

```mathematica
示例 1:

输入: "3+2*2"
输出: 7
示例 2:

输入: " 3/2 "
输出: 1
示例 3:

输入: " 3+5 / 2 "
输出: 5

说明：

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。
```



```javascript
解法一：利用栈来存储数，拿到后面的第二个值才判断符号计算乘除，负数的话前面加个-号，最后再对栈里面剩余的数求和，得到结果。
/**
 * @param {string} s
 * @return {number}
 */
var calculate = function(s) {
    s=s.replace(/ /g,'')
    let num=0,sign='+',stack=[],temps;//sign后被赋值,num为运算符后面的一个数字,sign存上一个运算符
    for(let i=0;i<=s.length;i++){ //还要进行最后一次运算，所以这里的次数加1
      temps=s[i];
      if(temps>= '0' && temps<='9'){
          num = num*10 + parseInt(temps);
          continue;
      }
      if(sign== '+'){
        stack.push(num)
      }else if(sign== '-'){
        stack.push(-num)
      }else if(sign == '*'){
        stack.push(stack.pop()*num)
      }else if(sign== '/'){
        stack.push(Math.trunc(stack.pop()/num)) //Math.trunc() 方法会将数字的小数部分去掉，只保留整数部分。
      }
      sign=temps; 
      num=0;
    }
  return stack.reduce((prev,cur)=>prev+cur)
};
```

### 459. 重复的子字符串（重做：1）

给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

```mathematica
示例 1:

输入: "abab"

输出: True

解释: 可由子字符串 "ab" 重复两次构成。
示例 2:

输入: "aba"

输出: False
示例 3:

输入: "abcabcabcabc"

输出: True

解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)

```

```javascript
解法一：s母串中如果符号条件的话，那么有一个特征就是，它是被各个相同的子串构成，s=s's's'。所以s+s两个拼接的话，肯定是在中间部分就有一个s。
/**
 * @param {string} s
 * @return {boolean}
 */
var repeatedSubstringPattern = function(s) {
  return (s+s).indexOf(s,1) != s.length
};
```



### 1370. 上升下降字符串

给你一个字符串 s ，请你根据下面的算法重新构造字符串：

从 s 中选出 最小 的字符，将它 接在 结果字符串的后面。
从 s 剩余字符中选出 最小 的字符，且该字符比上一个添加的字符大，将它 接在 结果字符串后面。
重复步骤 2 ，直到你没法从 s 中选择字符。
从 s 中选出 最大 的字符，将它 接在 结果字符串的后面。
从 s 剩余字符中选出 最大 的字符，且该字符比上一个添加的字符小，将它 接在 结果字符串后面。
重复步骤 5 ，直到你没法从 s 中选择字符。
重复步骤 1 到 6 ，直到 s 中所有字符都已经被选过。
在任何一步中，如果最小或者最大字符不止一个 ，你可以选择其中任意一个，并将其添加到结果字符串。

请你返回将 s 中字符重新排序后的 结果字符串 。

```mathematica
示例 1：

输入：s = "aaaabbbbcccc"
输出："abccbaabccba"
解释：第一轮的步骤 1，2，3 后，结果字符串为 result = "abc"
第一轮的步骤 4，5，6 后，结果字符串为 result = "abccba"
第一轮结束，现在 s = "aabbcc" ，我们再次回到步骤 1
第二轮的步骤 1，2，3 后，结果字符串为 result = "abccbaabc"
第二轮的步骤 4，5，6 后，结果字符串为 result = "abccbaabccba"
示例 2：

输入：s = "rat"
输出："art"
解释：单词 "rat" 在上述算法重排序以后变成 "art"
示例 3：

输入：s = "leetcode"
输出："cdelotee"
示例 4：

输入：s = "ggggggg"
输出："ggggggg"
示例 5：

输入：s = "spo"
输出："ops"
 

提示：

1 <= s.length <= 500
s 只包含小写英文字母。

```



```javascript
解法一：桶计数，用数组h[]，存放26个字母中出现的次数，再模拟题意，从小到大，就是从'a'-'z'，从大到小，就是从‘z'到‘a',当结果长度和s长度相等时，结束。注意：这里记录次数时，为找对应下标，用到了字符串的codePointAt()函数，返回 一个 Unicode 编码点值的非负整数。也就是ASCII码值返回回来了。
/**
 * @param {string} s
 * @return {string}
 */
var sortString = function (s) {
  let ret = '';
  const words = 'abcdefghijklmnopqrstuvwxyz';
  let h = new Array(26).fill(0);
  for (let i of s) {  //可以这样遍历字符串
    h[i.codePointAt() - 'a'.codePointAt()]++;
  }
  let slength = s.length;
  function appendStr(i) {
    if (h[i]) {
      h[i]--;
      ret += words[i];
    }
  }
  while (true) {
    if (slength == ret.length) break;
    for (let i = 0; i < 26; i++) appendStr(i);
    for (let i = 25; i >= 0; i--) appendStr(i);
  }
  return ret;
};

解法二：直接模拟题意，找最小找最大。但是这样做性能很差
/**
 * @param {string} s
 * @return {string}
 */
var sortString = function (s) {
  s= s.split('').map(item=>item.codePointAt());
  let ret= ''
  while(s.length){
  let min=-Infinity,max=Infinity;
    while(true){
      min = Math.min(...s.filter(item=> item>min))//给定数值中最小的数。如果任一参数不能转换为数值，则返回NaN。如果没有参数，结果为Infinity。
      if(min === Infinity) break;
      ret += String.fromCharCode(min);//返回由指定的 UTF-16 代码单元序列创建的字符串。
      s.splice(s.indexOf(min),1);  //删除这个选出的元素
    }
    while(true){
      max = Math.max(...s.filter(item=> item<max))
      if(max === -Infinity) break;
      ret += String.fromCharCode(max);
      s.splice(s.indexOf(max),1);  //删除这个选出的元素
    }
  }
  return ret;
};
```

### 面试题 01.06. 字符串压缩（重做：1）

字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串aabcccccaaa会变为a2b1c5a3。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。

```mathematica
示例1:

 输入："aabcccccaaa"
 输出："a2b1c5a3"
示例2:

 输入："abbccd"
 输出："abbccd"
 解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。
提示：

字符串长度在[0, 50000]范围内。


```



```javascript
解法一：模拟题意
/**
 * @param {string} S
 * @return {string}
 */
var compressString = function(S) {
    if(!S) {
        return '';
    }

    let result = S[0], sum = 1;
    for(let i = 1; i < S.length; ++i) {
        if(S[i] === S[i - 1]) {
            sum++;
        }else{
            result += sum;
            result += S[i];
            sum = 1;
        }
    }
    result += sum;
    return result.length >= S.length ? S : result;
};
```



### 468. 验证IP地址

编写一个函数来验证输入的字符串是否是有效的 IPv4 或 IPv6 地址。

IPv4 地址由十进制数和点来表示，每个地址包含4个十进制数，其范围为 0 - 255， 用(".")分割。比如，172.16.254.1；

同时，IPv4 地址内的数不会以 0 开头。比如，地址 172.16.254.01 是不合法的。

IPv6 地址由8组16进制的数字来表示，每组表示 16 比特。这些组数字通过 (":")分割。比如,  2001:0db8:85a3:0000:0000:8a2e:0370:7334 是一个有效的地址。而且，我们可以加入一些以 0 开头的数字，字母可以使用大写，也可以是小写。所以， 2001:db8:85a3:0:0:8A2E:0370:7334 也是一个有效的 IPv6 address地址 (即，忽略 0 开头，忽略大小写)。

然而，我们不能因为某个组的值为 0，而使用一个空的组，以至于出现 (::) 的情况。 比如， 2001:0db8:85a3::8A2E:0370:7334 是无效的 IPv6 地址。

同时，在 IPv6 地址中，多余的 0 也是不被允许的。比如， 02001:0db8:85a3:0000:0000:8a2e:0370:7334 是无效的。

说明: 你可以认为给定的字符串里没有空格或者其他特殊字符。

```mathematica
示例 1:

输入: "172.16.254.1"

输出: "IPv4"

解释: 这是一个有效的 IPv4 地址, 所以返回 "IPv4"。
示例 2:

输入: "2001:0db8:85a3:0:0:8A2E:0370:7334"

输出: "IPv6"

解释: 这是一个有效的 IPv6 地址, 所以返回 "IPv6"。
示例 3:

输入: "256.256.256.256"

输出: "Neither"

解释: 这个地址既不是 IPv4 也不是 IPv6 地址。

```

```js
解法一：模拟题意进行判断，适当的用到正则。注意"192.0.0.1"这种情况是合法的，IPv4 地址内的数不会以 0 开头的条件是多位数时。加个判断即可
/**
 * @param {string} IP
 * @return {string}
 */

var validIPAddress = function (IP) {
  if (IP.includes('.')) {
    return isIPv4(IP)
  } else {
    return isIPv6(IP)
  }
};
function isIPv4(IP) {
  IP = IP.split('.')
  for (let i of IP) {
    if (IP.length!==4 ||(i.length!==1 && i.startsWith('0')) || parseInt(i) > 255 || parseInt(i) < 0 || i.length > 4 || i.length <1 || /[a-z]|[A-Z]/g.test(i)) {
      return "Neither";
    }
  }
  return "IPv4"
}
function isIPv6(IP) {
  IP = IP.split(':')
  for (let i of IP) {
    if (IP.length!==8 ||i.length > 4 || i.length <1 || /[g-z]|[G-Z]/g.test(i) ) {
      return "Neither";
    }
  }
  return "IPv6"
}
```

### 1374. 生成每种字符都是奇数个的字符串

给你一个整数 n，请你返回一个含 n 个字符的字符串，其中每种字符在该字符串中都恰好出现 奇数次 。

返回的字符串必须只含小写英文字母。如果存在多个满足题目要求的字符串，则返回其中任意一个即可。

```mathematica
示例 1：

输入：n = 4
输出："pppz"
解释："pppz" 是一个满足题目要求的字符串，因为 'p' 出现 3 次，且 'z' 出现 1 次。当然，还有很多其他字符串也满足题目要求，比如："ohhh" 和 "love"。
示例 2：

输入：n = 2
输出："xy"
解释："xy" 是一个满足题目要求的字符串，因为 'x' 和 'y' 各出现 1 次。当然，还有很多其他字符串也满足题目要求，比如："ag" 和 "ur"。
示例 3：

输入：n = 7
输出："holasss"
 

提示：

1 <= n <= 500

```

```javascript
解法一：乍一看有些复杂，仔细一想，不就是奇数，根据输入的n判断奇偶，用'a''b'搭配就好。字符串的repeat方法
/**
 * @param {number} n
 * @return {string}
 */
var generateTheString = function(n) {
  let a='a';
  let b='b';
  let str='';
  if(n%2==0){
    str=a.repeat(n-1) +b;
  }else{
    str = a.repeat(n);
  }
  return str;
};
```

### 859. 亲密字符串

给定两个由小写字母构成的字符串 `A` 和 `B` ，只要我们可以通过交换 `A` 中的两个字母得到与 `B` 相等的结果，就返回 `true` ；否则返回 `false` 。

```mathematica
示例 1：

输入： A = "ab", B = "ba"
输出： true
示例 2：

输入： A = "ab", B = "ab"
输出： false
示例 3:

输入： A = "aa", B = "aa"
输出： true
示例 4：

输入： A = "aaaaaaabc", B = "aaaaaaacb"
输出： true
示例 5：

输入： A = "", B = "aa"
输出： false
 

提示：

0 <= A.length <= 20000
0 <= B.length <= 20000
A 和 B 仅由小写字母构成。

```

```javascript
解法一：两种情况，一个是A===B时，只要有两个相同字母即可实现交换，true，否则false。A！==B时，那么必有2个位置的字母不一样，否则false。
/**
 * @param {string} A
 * @param {string} B
 * @return {boolean}
 */
var buddyStrings = function (A, B) {
  if (A.length !== B.length) return false;
  if (A === B) {
    if ([...new Set(A.split(''))].length === A.length) { //判断是否有两个相同字母，有则true，无则false
      return false;
    } else {
      return true;
    }
  } else {
    let first = -1, second = -1;
    for (let i = 0; i < A.length; i++) {
      if (A[i] !== B[i]) {
        if (first === -1) {
          first = i;
        } else if (second === -1) {
          second = i;
        } else {
          return false;
        }
      }
    }
    return second != -1 && A[first] === B[second] && A[second] === B[first];
  }
};
```



### 415. 字符串相加(类似大数相加)(重做：1)

给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

 

提示：

num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
**你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式**

```js
解法一：类似大整数相加了， 先补0对齐。
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var addStrings = function (num1, num2) {
    // 先补0对齐
    while (num1.length > num2.length) num2 = '0' + num2;
    while (num1.length < num2.length) num1 = '0' + num1;
    let carry = 0, res = '', sum = 0;
    for (let i = num1.length - 1; i >= 0; i--) {
        sum = +num1[i] + +num2[i] + carry;
        res = sum % 10 + res;
        carry = sum > 9 ? 1 : 0;
    }
    return carry == 1 ? '1' + res : res;
};
```



### 67. 二进制求和（重做：1）

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 非空 字符串且只包含数字 1 和 0。

 ```mathematica
示例 1:

输入: a = "11", b = "1"
输出: "100"
示例 2:

输入: a = "1010", b = "1011"
输出: "10101"


提示：

每个字符串仅由字符 '0' 或 '1' 组成。
1 <= a.length, b.length <= 10^4
字符串如果不是 "0" ，就都不含前导零。


 ```

```js
解法一：类似上面字符串相加，不过将十进制换成了二进制罢了
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function (a, b) {
    let num1 = a, num2 = b;
    // 先补0对齐
    while (num1.length > num2.length) num2 = '0' + num2;
    while (num1.length < num2.length) num1 = '0' + num1;
    let carry = 0, res = '', sum = 0;
    for (let i = num1.length - 1; i >= 0; i--) {
        sum = +num1[i] + +num2[i] + carry;
        res = sum % 2 + res;
        carry = sum > 1 ? 1 : 0;
    }
    return carry == 1 ? '1' + res : res;
};
```



### 8. 字符串转换整数 (atoi)（重做：1，*）

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

```mathematica
示例 1:

输入: "42"
输出: 42
示例 2:

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:

输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。

```

```js
解法一：模拟呗。
/**
 * @param {string} str
 * @return {number}
 */
var myAtoi = function (str) {

    const myParseInt = (str) => {
        str = str.trim();
        let sign = 1, i = 0, result = 0;
        let firstChar = str[i];

        if (firstChar === '+') {
            i++;
        } else if (firstChar === '-') {
            sign = -1;
            i++;
        } else if (!/[0-9]/.test(firstChar)) {
            return NaN;
        }

        for (let k = i; k < str.length; ++k) {
            if (!/[0-9]/.test(str[k])) {
                return sign * result;
            }
            result = result * 10 + +str[k];
        }
        return sign * result;
    }
    
    const num = myParseInt(str);
    // const num = parseInt(str); √
    if (isNaN(num))
        return 0
    if (num > Math.pow(2, 31) - 1)
        return Math.pow(2, 31) - 1
    if (num < Math.pow(-2, 31))
        return Math.pow(-2, 31)
    return num
};

```



### 14. 最长公共前缀（重做：1，*）

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

```mathematica
示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。
。
```

```js
解法一：拿个最小的字符串长度，两层遍历，完事。模拟
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function (strs) {
    if (!strs.length) {
        return '';
    }

    let result = '';
    const minLength = strs.reduce((pre, cur) => Math.min(pre, cur.length), Infinity);
    
    for (let i = 0; i < minLength; ++i) {
        const char = strs[0][i];
        for (const str of strs) {
            if (str[i] !== char) {
                return result;
            }
        }
        result += char;
    }
    return result;
};
```



### 6. Z 字形变换（重做：1，*）

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

```mathematica
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G


```

```js
解法一：模拟。
动画可参考：https://leetcode-cn.com/problems/zigzag-conversion/solution/zzi-xing-bian-huan-by-jyd/
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function(s, numRows) {
    if(numRows === 1) {
        return s;
    }

    const rows = new Array(numRows).fill('');

    let curRow = 0, direction = -1;//这里的-1是配合下面进来就反向为正的一个初始化
    
    for(let i = 0; i < s.length; ++i) {
        rows[curRow] += s[i];
		//变向
        if(curRow === numRows - 1 || curRow === 0) {
            direction *= -1;
        }
        curRow += direction;
    }

    return rows.join('');
};
```



## 栈和队列

![image-20201018211547318](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201018211547318.png)

### 20. 有效的括号（重做：1）

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

```mathematica
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

```javascript
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

```



### 155. 最小栈（重做：1）

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

```mathematica
示例:

输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
 

提示：

pop、top 和 getMin 操作总是在 非空栈 上调用。
```

```javascript
解法一：用另外一个最小值栈保存最小值，注意这里不能仅用一个最小值变量去存，因为有pop操作会将原来的值pop出去，而getMin的范围最终只能在原数组里面
/**
 * initialize your data structure here.
 */
var MinStack = function () {
  this.size = 0;
  this.arr = [];
  this.minArr = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function (x) {
  this.stack[this.size] = x;
  this.minStack[this.size] = (this.size ? Math.min(x, this.getMin()) : x);
  this.size++;
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  this.size--;
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.size - 1];
};


/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
  return this.minStack[this.size - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```



### 146. LRU缓存机制（重做：1）

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

 

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

```javascript
示例:

LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4

```

```javascript
解法一：利用 Map 能记住添加的键值对的顺序
/**
 * @param {number} capacity
 */
var LRUCache = function(capacity) {
    // 利用 Map 能记住添加的键值对的顺序
    this.cache = new Map();
    this.capacity = capacity;
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
    const {cache} = this;
    if(cache.has(key)) {
        const value = cache.get(key);
        cache.delete(key);
        cache.set(key, value);
        return value;
    } else {
        return -1;
    }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    const {cache, capacity} = this;
    if(cache.has(key)) {
        cache.delete(key);
    } else if(cache.size >= capacity){
        cache.delete(cache.keys().next().value); // 删除map的第一个元素
    }
    cache.set(key, value);
};

/** 
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */

解法二：这题主要需要解决的就是如何在写入新数据之前删除最久未使用的数据值。get和put的操作都可以看做是使用了，那么这里使用队列，每次使用了就filter过滤下队列，然后将当前key，push到队列最后。从而实现更新。

/**
 * @param {number} capacity
 */
var LRUCache = function (capacity) {
  this.capacity = capacity;
  this.num = 0;
  this.cache = {};
  this.use = [];
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function (key) {
  if (this.cache[key]) {
    this.use = this.use.filter(item => item != key);
    this.use.push(key);
    return this.cache[key];
  } else {
    return -1;
  }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function (key, value) {
  if (this.cache[key]) {
    this.use = this.use.filter(item => item != key);
    this.use.push(key);
    this.cache[key]= value;
  } else {
    this.use.push(key);
    this.cache[key]= value;
    this.num++;
    if (this.num > this.capacity) {
      const key = this.use.shift();
      delete this.cache[key];
    }
  }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```



### 394. 字符串解码(重做：1，*)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

```mathematica
示例 1：

输入：s = "3[a]2[bc]"
输出："aaabcbc"
示例 2：

输入：s = "3[a2[c]]"
输出："accaccacc"
示例 3：

输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
示例 4：

输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"


```

![image-20201027113221947](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201027113221947.png)

```javascript
解法一：利用两个栈，分别存数字和字符。
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function(s) {
  let numStack = [];    //存数字的栈
  let strStack = [];    //存字符的栈
  let res = "";         //字符串的搬运工，临时存放位置
  let num = 0;         //数字的搬运工，临时存放位置
  for (const i of s) {
    if (!isNaN(i)) { //如果为数字
      num = num * 10 + Number(i);
    } else if (i === '[') {
      strStack.push(res);
      res = '';
      numStack.push(num);
      num = 0;
    } else if (i === ']') {
      const numMutil = numStack.pop();
      res = strStack.pop() + res.repeat(numMutil);
    } else {
      res += i;
    }
  }
  return res;
};
```



### 32. 最长有效括号（重做：1,*）

给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

```mathematica
示例 1:

输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
示例 2:

输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"

```

```js
解法一：动态规划
以")()())"为例

状态： dp[i] 表示以 s[i] 结尾的最长有效子串的长度

转移方程：dp[i] = dp[i-1] + 2 + dp[i-dp[i-1]-2] (当s[i - dp[i-1] - 1] === '(' && s[i] === ')' 时)
 		否则  dp[i] = 0

var longestValidParentheses = function (s) {
    const n = s.length;
    if (!n) {
        return 0;
    }

    const dp = new Array(n).fill(0);
    let result = 0;

    for (let i = 1; i < n; ++i) {
        if (s[i] === ')' && s[i - dp[i - 1] - 1] === '(') {
            dp[i] = dp[i - 1] + 2;
            // 如果匹配到的 '(' 前面还有有效长度的话，也加上
            if (i - dp[i - 1] - 2 > 0) {
                dp[i] += dp[i - dp[i - 1] - 2];
            }   
      //或者直接上面用一行：dp[i] = dp[i - 1] + 2 + (dp[i - dp[i - 1] - 2] || 0);

            result = Math.max(result, dp[i]);
        }
    }
    return result;
};

解法二:栈
详解：https://leetcode-cn.com/problems/longest-valid-parentheses/solution/shou-hua-tu-jie-zhan-de-xiang-xi-si-lu-by-hyj8/

// 用栈来解
var longestValidParentheses = function (s) {
    const n =s.length;
    const stack = [];
    stack.push(-1);
    let maxLen=0;
    for(let i =0; i <n;i++){
        if(s[i]==='('){
            stack.push(i)
        }else{
            stack.pop();
            if(stack.length===0){
                stack.push(i);
            }else{
                maxLen = Math.max(maxLen,i - stack[stack.length-1]);
            }
        }
    }
    return maxLen;
};



```



## 双指针

### 11. 盛最多水的容器（重做：1）

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg" alt="img" style="zoom:50%;" />

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。



```mathematica
示例：
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

```js
解法一：利用双指针，从两头遍历，用左右高度高低来做判断
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
 let i=0,j=height.length-1,maxOfArea=0,tempArea=0;
 while(i<j){
   if(height[i]<height[j]){   //当左边的高度低时
     tempArea=height[i]*(j-i);//高度取决于短的那方
     i++;                     //去测左边的右边一个高度情况
   }else{
    tempArea=height[j]*(j-i);
    j--;
   }
   maxOfArea=Math.max(maxOfArea,tempArea)
 }
 return maxOfArea
};
```



### 84. 柱状图中最大的矩形（重做：1）
给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

![image-20201110210252381](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201110210252381.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。

![image-20201110210318480](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201110210318480.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。

 ```mathematica
示例:

输入: [2,1,5,6,2,3]
输出: 10
 ```

```javascript
解法一：双指针，i作为从第一个柱子开始依次向后递增，j则更像是一个哨兵，每次i固定后，j就从i后面的柱子里面看可能性，这里比较巧妙也比较符合题意的就是，每次的i固定后，先保存其高度，再用j哨兵去依次的向右探测，用最低的身高去算面积，面积最大的保存下来。

/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function (heights) {
    let maxArea = 0;
    const n = heights.length;

    for (let i = 0; i < n; i++) {
        //每一个柱子，都和其后面的轮询一轮看结果
        let minHeight = heights[i];
        for (let j = i; j < n; j++) {//这里从i开始是因为还要看起本身的一个面积
            minHeight = Math.min(minHeight, heights[j]);
            maxArea = Math.max(maxArea, minHeight * (j - i + 1));
        }
    }
    return maxArea;
};
```



### 42. 接雨水（重做：1）

给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 ![image-20201217004603118](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201217004603118.png)

```mathematica
示例 1：
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

示例 2：

输入：height = [4,2,0,3,2,5]
输出：9
 

提示：

n == height.length
0 <= n <= 3 * 104
0 <= height[i] <= 105

```

```mathematica
解法一：双数组
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function (height) {
    let maxHeight = 0;
    let volume = 0;
    let leftMax = [];
    let rightMax = [];
    //leftMax[i]代表 i 的左侧柱子的最大值
    for (let i = 0; i < height.length; i++) {
        leftMax[i] = maxHeight = Math.max(maxHeight, height[i]);
    }
    maxHeight=0;
    //rightMax[i]代表 i 的左侧柱子的最大值
    for (let i = height.length - 1; i >= 0; i--) {
        rightMax[i] = maxHeight = Math.max(maxHeight, height[i]);
    }

    for (let i = 0; i < height.length; i++) {
        volume += Math.min(leftMax[i], rightMax[i]) - height[i];
    }
    return volume;
};
```



## 搜索（回溯，BFS，DFS）

### 17. 电话号码的字母组合（重做：1）

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![image-20200917200850233](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200917200850233.png)



```mathematica
示例:

输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

```

![image-20201123203829172](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201123203829172.png)

```javascript
解法一：回溯
/*
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function (digits) {
  if (!digits) {
    return [];
  }
  const ret = [];
  const len = digits.length;
  const mp ={ '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz' };

  function dfs(str, i) {
    if (i == len) {
      ret.push(str);
      return
    }
    const temp = mp[digits[i]];
    for (let j = 0; j < temp.length; j++) {
      dfs(str + temp[j], i+1);
    }
  }
  dfs('',0);
  return ret;
};
```



### 78. 子集（重做：1）

给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

```mathematica
示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

![image-20200920155905202](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200920155905202.png)

```javascript
解法一：回溯法
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
function backTrack(list, tempList, nums, start) {
  list.push([...tempList]);
  for (let i = start; i < nums.length; i++) {
    tempList.push(nums[i]);
    backTrack(list,tempList,nums,i+1);
    tempList.pop();//回溯
  }
}
var subsets = function (nums) {
  const list = [];
  backTrack(list, [], nums, 0);
  return list;
};
```



### 90. 子集 II（重做：1）

给定一个可能包含重复元素的整数数组 ***nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

```mathematica
示例:

输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



```javascript
解法一：回溯+去重。注意，题意这里结果中的[1,2,2],和[2,1,2]是一样的，得去掉，也算作重复的。
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

function backTrack(list, tempList, nums, start) {
  list.push([...tempList]);
  for (let i = start; i < nums.length; i++) {
    tempList.push(nums[i]);
    backTrack(list,tempList,nums,i+1);
    tempList.pop();//回溯
  }
}
var subsetsWithDup = function (nums) {
  const list = [];
  backTrack(list, [], nums, 0);
  const res={};
  list.map(item=>{
    item.sort((v1,v2)=>v1-v2);
    res[item]=item;
  })
  return Object.values(res);
};
```



### 39. 组合总和（重做：1）

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

```mathematica
示例 1：

输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
示例 2：

输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
 

提示：

1 <= candidates.length <= 30
1 <= candidates[i] <= 200
candidate 中的每个元素都是独一无二的。
1 <= target <= 500

```

```javascript
解法一：使用通用回溯法。将图在脑海中稍微描述下。
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
function backTrack(list, tempList, candidates, start,target) {
  if(tempList.length&&tempList.reduce((prev, cur) => prev + cur) > target) return
  if (tempList.length&&tempList.reduce((prev, cur) => prev + cur) === target) {
    list.push([...tempList]);
  }
  for (let i = start; i < candidates.length; i++) {
    tempList.push(candidates[i]);
    backTrack(list,tempList,candidates,i,target);// 数字可以重复使用， i + 1代表不可以重复利用
    tempList.pop();
  }
}
var combinationSum = function (candidates, target) {
  let list = [];
  backTrack(list, [], candidates,0,target);

  return list     //这里上面的循环是传递的i进去的，而不是最开始的默认一直是传递0，所以之前会出现[2,2,3]和[2,3,2]的组合等于7需要去重，但是上面的循环传递的i，所以就不需要考虑二维去重了
};


解法二：用差值是否等于0去判断
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
function backtrack(list, tempList, nums, remain, start) {
  if (remain < 0) return;
  else if (remain === 0) return list.push([...tempList]);
  for (let i = start; i < nums.length; i++) {
    tempList.push(nums[i]);
    backtrack(list, tempList, nums, remain - nums[i], i); // 数字可以重复使用， i + 1代表不可以重复利用
    tempList.pop();
  }
}
var combinationSum = function(candidates, target) {
  const list = [];
  backtrack(list, [], candidates.sort((a, b) => a - b), target, 0);
  return list;
};
```



### 40. 组合总和 II（重做：1）

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明：

所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 

```mathematica
示例 1:

输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
示例 2:

输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]

```



```javascript
解法一：使用通用回溯法。将图在脑海中稍微描述下。这题和39稍微有两点不一样，第一是数不能重复利用，则需要传递i+1。第二是数组里面可以有重复数字，这意味着在后面的解集里面需要进行二维数组的去重
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
function backTrack(list, tempList, candidates, start,target) {
  if(tempList.length&&tempList.reduce((prev, cur) => prev + cur) > target) return
  if (tempList.length&&tempList.reduce((prev, cur) => prev + cur) === target) {
    list.push([...tempList]);
  }
  for (let i = start; i < candidates.length; i++) {
    tempList.push(candidates[i]);
    backTrack(list,tempList,candidates,i+1,target);// i + 1代表不可以重复利用
    tempList.pop();
  }
}
var combinationSum = function (candidates, target) {
  let list = [];
  backTrack(list, [], candidates,0,target);
  //对二维数字数组去重
  let res={};
  list.map((item)=>{
    item.sort((v1,v2)=>v1-v2);
    res[item]=item
  })
  return Object.values(res);

};
```



### 46. 全排列（重做：1）

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

```mathematica
示例:
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



```javascript
解法一：使用通用回溯法。将图在脑海中稍微描述下。
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

function backTrack(list, tempList, nums) {
  if (tempList.length === nums.length) {
    list.push([...tempList]);
    return;
  }
  for (let i = 0; i < nums.length; i++) {
    if (!tempList.includes(nums[i])) {//数组里面无重复的值，所以回溯时如果碰到相同的值就不做处理，不同的才加进来
      tempList.push(nums[i]);
      backTrack(list, tempList, nums);
      tempList.pop();
    }
  }
}
var permute = function (nums) {
  const list = [];
  backTrack(list, [], nums, 0);
  return list;
};

解法二：用visited数组标记是否用过，和全排列2的解法类似
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

function backtrack(list, nums, tempList, visited) {
  if (tempList.length === nums.length) return list.push([...tempList]);
  for (let i = 0; i < nums.length; i++) {
    // 我们需要过滤这种情况
    if (visited[i]) continue;

    visited[i] = true;
    tempList.push(nums[i]);
    backtrack(list, nums, tempList, visited);
    visited[i] = false;
    tempList.pop();
  }
}
var permute = function (nums) {
  const list = [];
  backtrack(list, nums.sort((v1, v2) => v1 - v2), [], []);
  return list;
};
```



### 47. 全排列 II（重做：1）

给定一个可包含重复数字的序列，返回所有不重复的全排列。

```mathematica
示例:

输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```



```javascript
解法一：回溯，和46.permutations的区别是这道题的nums是可以重复的
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
function backtrack(list, nums, tempList, visited) {
  if (tempList.length === nums.length) return list.push([...tempList]);
  for (let i = 0; i < nums.length; i++) {
    // 我们需要过滤这种情况
    if (visited[i]) continue;
    // visited[i - 1] 这个判断容易忽略。如果没有这句，则实例会是[[1,1,2],[1,2,1],[1,1,2],[1,2,1],[2,1,1],[2,1,1]]
    if (i > 0 && nums[i] === nums[i - 1] && visited[i - 1]) continue;

    visited[i] = true;
    tempList.push(nums[i]);
    backtrack(list, nums, tempList, visited);
    visited[i] = false;
    tempList.pop();
  }
}

var permuteUnique = function (nums) {
  const list = [];
  backtrack(list, nums.sort((v1, v2) => v1 - v2), [], []);
  return list;
};
```



### 112. 路径总和（重做：1）

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**说明:** 叶子节点是指没有子节点的节点。

```mathematica
示例: 
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。

```

```js
解法一：递归搜索。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {boolean}
 */
var hasPathSum = function(root, sum) {
    if(!root) {
        return false;
    }

    if(!root.left && !root.right) {
        return sum === root.val;
    }

    return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
};

解法二：用113路径综合2 回溯的思想。
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {boolean}
 */
function backTrack(list, tempList, root, sum) {
  if (root == null) return;
  tempList.push(root.val);  //前序遍历，根、左、右
  if (root.left == null && root.right == null && tempList.length && tempList.reduce((prev, cur) => prev + cur) === sum) {
    list.push([...tempList]);
    return;
  }
  backTrack(list, tempList, root.left, sum)
  backTrack(list, tempList, root.right, sum)
  tempList.pop();
}
var hasPathSum = function (root, sum) {
  const list = [];
  backTrack(list, [], root, sum)
  return list.length > 0;
};
```



### 113. 路径总和 II（重做：1）

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**说明:** 叶子节点是指没有子节点的节点。

```mathematica
示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]


```

```javascript
	
验证二叉搜索树解法一：回溯+前序遍历
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {number[][]}
 */
function backTrack(list, tempList, root, sum) {
  if (root == null) return;
  tempList.push(root.val);  //前序遍历，根、左、右
  if (root.left == null && root.right == null && tempList.length && tempList.reduce((prev, cur) => prev + cur) === sum) {
    list.push([...tempList]);
  }
  backTrack(list, tempList, root.left, sum)
  backTrack(list, tempList, root.right, sum)
  tempList.pop();
}
var pathSum = function (root, sum) {
  const list = [];
  backTrack(list, [], root, sum)
  return list;
};
```



### 437. 路径总和 III（重做：1）

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

```mathematica
示例：

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

```javascript
解法一：两个递归。空间复杂度O(n) 时间复杂度介于O(nlogn) 和 O(n^2)
1.一个递归求从任意一个节点开始有多少条满足条件的路径。
2.一个递归是遍历整棵树，求各个节点开始各有多少满足条件的路径数，各路径数相加即可得出符合条件的总路径数。

//一个递归求从任意一个节点开始有多少条满足条件的路径。
function findSum(root, rest) {
  if (!root) return 0;
  const res = root.val === rest ? 1 : 0;
  const L = findSum(root.left, rest - root.val);
  const R = findSum(root.right, rest - root.val);
  return L + R + res;
}

//一个递归是遍历整棵树，求各个节点开始各有多少满足条件的路径数，各路径数相加即可得出符合条件的总路径数
var pathSum = function (root, sum) {
  if (!root) return 0;
  const rootRes = findSum(root, sum);
  const rootLeftRes = pathSum(root.left, sum);
  const rootRightRes = pathSum(root.right, sum);

  return rootRes + rootLeftRes + rootRightRes;
};

解法二：哈希表、前缀和。
function helper(root, prefixSum, target, hashmap) {
  // see also : https://leetcode.com/problems/subarray-sum-equals-k/

  if (root === null) return 0;
  let count = 0;
  prefixSum += root.val;
  if (hashmap[prefixSum - target] ) {
    count += hashmap[prefixSum - target];
  }
  if (hashmap[prefixSum] === void 0) {
    hashmap[prefixSum] = 1;
  } else {
    hashmap[prefixSum]++;
  }
  const res =
    count +
    helper(root.left, prefixSum, target, hashmap) +
    helper(root.right, prefixSum, target, hashmap);

    // 这里要注意别忘记了,回溯
    hashmap[prefixSum]--;

    return res;
}

var pathSum = function(root, sum) {
  const hashmap = {0:1};
  return helper(root, 0, sum, hashmap);
};
```





### 131. 分割回文串

给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回 *s* 所有可能的分割方案。

```mathematica
示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```

![image-20200926205436268](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20200926205436268.png)

```javascript
解法一：回溯（主要是将流程图能够在脑海里有个印象，根据流程思路写代码）
/**
 * @param {string} s
 * @return {string[][]}
 */
function isPlalindrome(s) {
  return s.split('').reverse().join('') === s;
}
function backTrack(list, tempList, s, start) {
  const sliced = s.slice(start);
  if (isPlalindrome(sliced) && tempList.join('').length === s.length) {
    list.push([...tempList])
  }
  for (let i = 0; i < sliced.length; i++) {
    const subSliced = sliced.slice(0, i + 1);
    if (isPlalindrome(subSliced)) {
      tempList.push(subSliced);
      backTrack(list, tempList, s, start + i + 1);
      tempList.pop();
    }
  }
}
var partition = function (s) {
  const list = [];
  backTrack(list, [], s, 0);
  return list;
};
```



### 93. 复原IP地址（重做：1，*）

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。

```mathematica
示例 1：

输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
示例 2：

输入：s = "0000"
输出：["0.0.0.0"]
示例 3：

输入：s = "1111"
输出：["1.1.1.1"]
示例 4：

输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]
示例 5：

输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
 

提示：

0 <= s.length <= 3000
s 仅由数字组成

```

```js
解法一：回溯。
// 复原从start开始的子串
const backTrack = (list, tempList, s, start) => {
    if (tempList.length == 4 && start == s.length) {
        list.push(tempList.join('.'));
        return;
    }
    if (tempList.length == 4 && start < s.length) {  // 满4段，字符未耗尽，不用往下选了
        return;
    }
    for (let len = 1; len <= 3; len++) {           // 枚举出选择，三种切割长度
        if (start + len  >= s.length + 1) return;     // 主要是为了后面的slice不越界，不超过s的长度
        if (len != 1 && s[start] == '0') return;     // 不能切出'0x'、'0xx'，题目里的不能含有前导0

        const str = s.slice(start, start + len); // 当前选择切出的片段
        if (len == 3 && +str > 255) return;          // 不能超过255

        tempList.push(str);                            // 作出选择，将片段加入tempList
        backTrack(list, tempList, s, start + len);                    // 基于当前选择，继续选择，注意更新指针
        tempList.pop(); // 上面一句的递归分支结束，撤销最后的选择，进入下一轮迭代，考察下一个切割长度
    }
};


const restoreIpAddresses = (s) => {
    const list = [];
    backTrack(list, [], s, 0);       // dfs入口
    return list;
};
```



### 22. 括号生成（重做：1）

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

```javascript
示例：

输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

```javascript
解法一：//回溯（dfs搜索+剪枝）
/**
 * @param {number} n
 * @return {string[]}
 */
/*
 * @param l 左括号已经用了几个
 * @param r 右括号已经用了几个
*/
function dfs(l, r, n, str, list) {
  if (l == n && r == n) {
    list.push(str)
    return;
  }
  //当前如果右括号已经多于左括号了，则一定不适合，剪枝
  if (l < r) return;
  // l < n 时可以插入左括号，最多可以插入 n 个
  if (l < n) {
    dfs(l + 1, r, n, str + '(', list);
  }
  // r < l 时 可以插入右括号,其实用r < n也行，不过r<l的含义更好
  if (r < l) {
    dfs(l, r + 1, n, str + ')', list);
  }
}
var generateParenthesis = function (n) {
  const list = [];
  dfs(0, 0, n, '', list);
  return list;
};
```



### 79. 单词搜索（重做：1）

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过**相邻的单元格**内的字母构成，**其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。**

```mathematica
示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
 

提示：

board 和 word 中只包含大写和小写英文字母。
1 <= board.length <= 200
1 <= board[i].length <= 200
1 <= word.length <= 10^3

```

```javascript
解法一：dfs回溯。将经过的用适当的符号标记起来，可以用set或boolean二维数组，或者直接置为null。在这里我直接置为null
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */

function dfs(row, col, rows, cols, board, word, start) {
  if (row >= rows || row < 0) return false;
  if (col >= cols || col < 0) return false; 
  //配合前面的两个for循环，找第一个匹配的字符
  const item = board[row][col];
  if (item !== word[start]) return false;
    
  if (start + 1 == word.length) return true;

  //已经访问过的置位NULL
  board[row][col] = null;

  //上下左右分别看情况是否符合
  const res = 
    dfs(row - 1, col, rows, cols, board, word, start + 1) ||
    dfs(row + 1, col, rows, cols, board, word, start + 1) ||
    dfs(row, col - 1, rows, cols, board, word, start + 1) ||
    dfs(row, col + 1, rows, cols, board, word, start + 1);
  
  //回溯
  board[row][col] = item;
  return res;
}
var exist = function (board, word) {
  if (board.length == 0) return false;
  if (word.length == 0) return false;
  const rows = board.length;
  const cols = board[0].length;
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      const res = dfs(i, j, rows, cols, board, word, 0);
      if (res) return true;
    }
  }
  return false;
};
```



### 200. 岛屿数量（重做：1）

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

```mathematica
示例 1：

输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
示例 2：

输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
 

提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] 的值为 '0' 或 '1'

```

```javascript
解法一：DFS。二维数组DFS解题模板。将已经访问的元素置为0，省去visited的空间开销。
/**
 * @param {character[][]} grid
 * @return {number}
 */
function dfs(row, col, n, m, grid) {
  if (row < 0 || row > n || col < 0 || col > m || grid[row][col] === "0") return;

  grid[row][col] = "0";  //省去visited的空间开销
  dfs(row - 1, col, n, m, grid);
  dfs(row + 1, col, n, m, grid);
  dfs(row, col + 1, n, m, grid);
  dfs(row, col - 1, n, m, grid);
}
var numIslands = function (grid) {
  if (grid.length === 0) return 0;
  let res = 0;
  const n = grid.length-1;
  const m = grid[0].length-1;
  for (let i = 0; i <= n; i++) {
    for (let j = 0; j <= m; j++) {
      if (grid[i][j] == "1") {
        dfs(i, j, n, m, grid);
        res++;
      }
    }
  }
  return res;
};
```



### 695. 岛屿的最大面积（重做：1）

给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

```mathematica
示例 1:

[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。

示例 2:

[[0,0,0,0,0,0,0,0]]
对于上面这个给定的矩阵, 返回 0。
```

```js
解法一：和上题岛屿数量类似，只不过这题求的是一个最大面积的岛屿。DFS。二维数组DFS解题模板。
/**
 * @param {number[][]} grid
 * @return {number}
 */

var maxAreaOfIsland = function (grid) {
    if (grid.length === 0) return 0;
    let maxRes = 0;
    let res = 0;
    const n = grid.length - 1;
    const m = grid[0].length - 1;

    function dfs(row, col, n, m, grid) {
        if (row < 0 || row > n || col < 0 || col > m || grid[row][col] === 0) return;
        res++;
        grid[row][col] = 0;  //省去visited的空间开销
        dfs(row - 1, col, n, m, grid);
        dfs(row + 1, col, n, m, grid);
        dfs(row, col + 1, n, m, grid);
        dfs(row, col - 1, n, m, grid);
    }

    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= m; j++) {
            if (grid[i][j] == 1) {
                dfs(i, j, n, m, grid);
                maxRes= Math.max(maxRes,res);
                res=0;
            }
        }
    }
    return maxRes;
};
```



## 拓扑排序

### 207. 课程表

你这个学期必须选修 numCourse 门课程，记为 0 到 numCourse-1 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：[0,1]

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

```mathematica
示例 1:
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
示例 2:

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。

提示：

输入的先决条件是由 边缘列表 表示的图形，而不是 邻接矩阵 。详情请参见图的表示法。
你可以假定输入的先决条件中没有重复的边。
1 <= numCourses <= 10^5

```

```javascript
解法一:拓扑排序、BFS。让入度为 0 的课入列，它们是能直接选的课。逐个出列，出列代表着课被选，需要减小相关课的入度。如果相关课的入度新变为 0，安排它入列、再出列……直到没有入度为 0 的课可入列。
入度数组：课号 0 到 n - 1 作为索引，通过遍历先决条件表求出对应的初始入度。
邻接表：用哈希表记录依赖关系。key：课号。value：依赖这门课的后续课（数组）

/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function (numCourses, prerequisites) {
  let inDegree = new Array(numCourses).fill(0); //入度数组
  let myMap = {};     //领接哈希表，用来存课号和依赖这门课的后续课(数组)
  for (let i = 0; i < prerequisites.length; i++) {
    inDegree[prerequisites[i][0]]++; //计算每门课的入度值
    if (myMap[prerequisites[i][1]]) {
      myMap[prerequisites[i][1]].push(prerequisites[i][0]);
    } else {
      myMap[prerequisites[i][1]] = [prerequisites[i][0]];
    }
  }

  let queue = [];  //实现BFS，让入度为 0 的课入列，如果相关课的入度新变为 0，安排它入列、再出列……直到没有入度为 0 的课可入列。
  for (let i = 0; i < inDegree.length; i++) {
    if (inDegree[i] == 0) {
      queue.push(i);
    }
  }
  let count = 0;
  while (queue.length) {
    const selected = queue.shift(); //入度为0的课，选出来
    count++;                        // 选课数+1
    let toQueue = myMap[selected];  //入度为0的这门课的后续课(数组)
    if (toQueue && toQueue.length) {
      for (let i = 0; i < toQueue.length; i++) {
        inDegree[toQueue[i]]--;   // 依赖它的后续课的入度-1
        if (inDegree[toQueue[i]] == 0) { // 如果因此减为0，入列
          queue.push(toQueue[i]);
        }
      }
    }
  }
  return count == numCourses;
};
```



## 贪心

### 55. 跳跃游戏（重做：1）

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

```mathematica
示例 1:

输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
示例 2:

输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。

```

```javascript
//解法一：贪心。贪心建模(记录和更新当前位置能够到达的最大的索引即可)
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
  let maxPosition = 0;
  for (let i = 0; i < nums.length; i++){
    if (maxPosition < i) return false;//max都小于当前的i了，说明走不到后面的位置了
    maxPosition = Math.max(max, nums[i] + i);
  }
  return maxPosition >= nums.length - 1;
};

解法二：动态规划。优点是可以有一定的规律和方法解题，不像贪心算法那样对某些题是为唯一性的，但在这里动态规划效率较慢时间复杂度为O（N^2）
//状态dp[i]为是否能跳到i位置
//从j位置能否跳到i位置，由此得
//转移方程为 dp[i] =OR(dp[j]&&nums[j]+j>=i)
/**
 * @param {number[]} nums
 * @return {boolean}
 */
//状态dp[i]为是否能跳到i位置
//转移方程为 dp[i] =OR(dp[j]&&nums[j]+j>=i)
var canJump = function (nums) {
  let n = nums.length;
  const dp = [];
  dp[0] = true;
  for (let i = 1; i < n; i++) {
    dp[i] = false;
    for (let j = 0; j < i; j++) {
      if (dp[j] && nums[j] + j >= i) {
        dp[i] = true;
        break;
      }
    }
  }
  return dp[n - 1];
};
```



### 406. 根据身高重建队列（重做：1）

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。

注意：
总人数少于1100人。

```mathematica
示例

输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

```

![image-20201027150722072](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201027150722072.png)

```javascript
解法一：贪心算法。身高高的可以无视身高矮的，只要我身高高的排好序，中间或旁边添加再多比我身高矮的都不影响我结果。
1.将身高排序，降序，如果身高相同，升序（sort可以多重排序）
2.循环，将每个身高插入到对应的位置即可。
/**
 * @param {number[][]} people
 * @return {number[][]}
 */
var reconstructQueue = function(people) {
  let res = [];
  people.sort((v1, v2) => {
    if (v1[0] === v2[0]) {
      return v1[1] - v2[1];//身高相同，按k升序
    } else {
      return v2[0] - v1[0];//身高不同，按身高降序
    }
  })

  people.forEach((item) => {
    res.splice(item[1], 0, item);
  })
  return res;
};
```



## 动态规划

专业课中对于动态规划的概述：

1. 动态规划算法的基本思想：将待求解问题分解成若干个子问题,先求解子问题,然后从这些子问题的解得到原问题的解。

2. 动态规划经分解得到的子问题往往不是相互独立的，有大量子问题会重复出现。

3. 为了避免重复计算,动态规划法是用一个表来存放一计算过的子问题。

**通常按四步骤设计动态规划算法：**

**(1)找出最优解的性质，并刻画其结构特征；**

**(2)递归定义求最优值的公式；**

**(3)以自底向上方式计算最优值；**

**(4)根据计算最优值时得到的信息，构造最优解。**

![image-20201103193947060](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201103193947060.png)



![image-20201103194018616](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201103194018616.png)

![image-20201103194650906](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201103194650906.png)

### 坐标型

![image-20201105115023436](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201105115023436.png)

![image-20201105143640270](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201105143640270.png)



#### 62. 不同路径（重做：1）

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

![image-20201015210803720](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201015210803720.png)

```mathematica
示例 1:

输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
示例 2:

输入: m = 7, n = 3
输出: 28
 

提示：

1 <= m, n <= 100
题目数据保证答案小于等于 2 * 10 ^ 9
```

```javascript
解法一：动态规划。
状态方程就是dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
第一行和第一列初始转态均为1，
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
  //第一行和第一列初始转态均为1，
  const dp = new Array(m).fill(0).map(_ => new Array(n).fill(1));

  for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
      dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    }
  }
  return dp[m - 1][n - 1];
};


解法二：由解法一可知：从左上角到右下角的走法 = 从右边开始走的路径总数+从下边开始走的路径总数
下一行的值 = 当前行的值+上一行的值
dp[i][j] = dp[i-1][j]+dp[i][j-1]
<=> dp[j] = dp[j]+dp[j-1]

/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
  const dp = new Array(n).fill(1);
  for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
      dp[j] = dp[j - 1] + dp[j];
    }
  }
  return dp[n - 1];
};

```

#### 63. 不同路径 II（重做：1）

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

![image-20201106192650509](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201106192650509.png)

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

```mathematica
说明：m 和 n 的值均不超过 100。

示例 1:

输入:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
输出: 2
解释:
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右

```

```javascript
解法一:和上题类似的思路，但是有障碍的地方需要拎出来，并且第一行和第一列的初始条件也不能有了。
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */
var uniquePathsWithObstacles = function (obstacleGrid) {
    const n = obstacleGrid.length;
    if (n == 0) {
        return 0;
    }
    const m = obstacleGrid[0].length;
    if (m == 0) {
        return 0;
    }
    const dp = [];
    for (let i = 0; i < n; i++) {
        dp[i] = [];
    }

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < m; j++) {
            if (obstacleGrid[i][j] == 1) {
                dp[i][j] = 0;
                continue;
            }

            if (i == 0 && j == 0) {
                dp[i][j] = 1;//这个初始条件之所以不写在外面，是因为第一行和第一列其他的值我们并不能在开始就知道，因为也有可能存在障碍物
                continue;
            }

            dp[i][j] = 0;
            if (i > 0) {
                dp[i][j] += dp[i - 1][j]
            }
            if (j > 0) {
                dp[i][j] += dp[i][j - 1]
            }
        }
    }
    return dp[n - 1][m - 1];
};
```



#### 64. 最小路径和（重做：1）

给定一个包含非负整数的 *m* x *n* 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

```mathematica
示例:

输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

```javascript
解法一：动态规划。
状态：dp[i][j]为i,j位置的最小路径，那么上格的最小路径+grid[i][j]和左格的最小路径+grid[i][j]最小值为当前最小路径。
转移方程：dp[i][j] = grid[i][j] + Math.min(dp[i][j - 1], dp[i - 1][j]);
初始条件：dp[i][j] = grid[0][0];
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minPathSum = function (grid) {
    const n = grid.length;
    if (n == 0) {
        return 0;
    }
    const m = grid[0].length;
    if (m == 0) {
        return 0;
    }
    
    const dp = new Array(n).fill(0).map(_ => new Array(m));

    dp[0][0] = grid[0][0];
    for (let i = 1; i < n; ++i) {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
    }
    for (let j = 1; j < m; ++j) {
        dp[0][j] = dp[0][j - 1] + grid[0][j];
    }
    
    for (let i = 1; i < n; i++) {
        for (let j = 1; j < m; j++) {
            dp[i][j] = grid[i][j] + Math.min(dp[i][j - 1], dp[i - 1][j]);
        }
    }
    return dp[n - 1][m - 1];
};
```



#### 674. 最长连续递增序列（重做：1）

给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

```mathematica
示例 1：

输入：nums = [1,3,5,4,7]
输出：3
解释：最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 4 隔开。 
示例 2：

输入：nums = [2,2,2,2,2]
输出：1
解释：最长连续递增序列是 [2], 长度为1。
 

提示：

0 <= nums.length <= 104
-109 <= nums[i] <= 109

```

```javascript
解法一：动态规划。
状态：dp[i] 为以nums[i]结尾的最长的递增子序列。则dp[i-1]为以nums[i-1]结尾的最长的递增子序列

转移方程： dp[i] = Math.max(1, dp[i - 1] + 1);

初始条件：空，或者认为第一个为1，其实每个地方默认开始的都是为1，本身的长度

结果：Math.max(dp[i]); 0 <= i < n
/**
 * @param {number[]} nums
 * @return {number}
 */
var findLengthOfLCIS = function (nums) {
    const n = nums.length;
    if (n == 0) {
        return 0;
    }

    const dp = [];
    dp[0] = 1;

    for (let i = 1; i < n; i++) {
        dp[i] = 1;
        if (nums[i] > nums[i - 1]) {
            dp[i] = Math.max(dp[i], dp[i - 1] + 1);
        }
    }
    return Math.max(...dp)
};
```





#### 300. 最长上升子序列（重做：1）

给定一个无序的整数数组，找到其中最长上升子序列的长度。

```mathematica
示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。

进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

```

```javascript
解法一：动态规划。
状态：求以nums[i]结尾的最长子序列。则需要求其前面的某一个j位置nums[j]的最长子序列。

转移方程：dp[i] = Math.max(dp[i], dp[j] + 1); (nums[i]>nums[j])

初始条件：无。或者也可以认为第一位的长度为1.

结果，max(dp[0],dp[1],....,dp[n-1])

/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function (nums) {
    const n = nums.length;
    if (n == 0) {
        return 0;
    }
    const dp = [];
    dp[0] = 1;
    for (let i = 0; i < n; i++) {
        dp[i] = 1;//每个位置至少有自己本身的长度1
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    return Math.max(...dp)
};

解法二：将算法的时间复杂度降低到 O(n log n) 。
```



#### 1143.最长公共子序列

```mathematica
示例 1:

输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:

输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:

输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
 

提示:

1 <= text1.length <= 1000
1 <= text2.length <= 1000
输入的字符串只含有小写英文字符。
```

```js
解法一：动态规划。
/**
 * @param {string} text1
 * @param {string} text2
 * @return {number}
 */
// dp[i][j] = text1[i] === text2[j] ? dp[i-1][j-1] + 1 : Math.max(dp[i-1][j], dp[i][j-1])
var longestCommonSubsequence = function(text1, text2) {
    const len1 = text1.length, len2 = text2.length;
    if(!len1 || !len2) {
        return 0;
    }

    const dp = new Array(len1).fill(0).map(_ => new Array(len2).fill(0));

    dp[0][0] = text1[0] === text2[0] ? 1 : 0;

    for(let i = 1; i < len1; ++i) {
        dp[i][0] = dp[i-1][0] || text1[i] === text2[0] ? 1 : 0;
    }
    for(let j = 1; j < len2; ++j) {
        dp[0][j] = dp[0][j-1] || text1[0] === text2[j] ? 1 : 0;
    }

    for(let i = 1; i < len1; ++i) {
        for(let j = 1; j < len2; ++j) {
            dp[i][j] = text1[i] === text2[j] ? dp[i-1][j-1] + 1 : Math.max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return Math.max(...dp.map(item => Math.max(...item)));
};

```





#### 931. 下降路径最小和

给定一个方形整数数组 A，我们想要得到通过 A 的下降路径的最小和。

下降路径可以从第一行中的任何元素开始，并从每一行中选择一个元素。在下一行选择的元素和当前行所选元素最多相隔一列。

 ```mathematica
示例：

输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：12
解释：
可能的下降路径有：
[1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]
[2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]
[3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]
和最小的下降路径是 [1,4,7]，所以答案是 12。

 

提示：

1 <= A.length == A[0].length <= 100
-100 <= A[i][j] <= 100

 ```

```javascript
解法一：动态规划。
状态：dp[i][j]为当前节点的最小和。那么，dp[i - 1][j]，dp[i - 1][j - 1]，dp[i - 1][j + 1]为它上一行的三个最小和，分别是上格，左上角，右上角。

转移方程：  dp[i][j] = Math.min(dp[i - 1][j] + A[i][j]，dp[i - 1][j - 1] + A[i][j],dp[i - 1][j + 1] + A[i][j]);

初始条件:第一行的最小和为A[0][j]。

答案：最后一行里面找最小和
/**
 * @param {number[][]} A
 * @return {number}
 */
var minFallingPathSum = function (A) {
    // 1 2 3
    // 4 5 6
    // 7 8 9
    const n = A.length;
    if (n == 0) {
        return 0;
    }

    const m = A[0].length;
    if (m == 0) {
        return 0;
    }

    const dp = [];
    for (let i = 0; i < n; i++) {
        dp[i] = [];
    }
    let res = Infinity;
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < m; j++) {
            if (i == 0) {
                dp[i][j] = A[i][j];
                continue;
            }
            dp[i][j] = Infinity;
            //当前节点的上格最小和
            if (i > 0) {
                dp[i][j] = Math.min(dp[i - 1][j] + A[i][j], dp[i][j]);
            }
            //当前节点的左上格最小和
            if (i > 0 && j > 0) {
                dp[i][j] = Math.min(dp[i - 1][j - 1] + A[i][j], dp[i][j]);
            }
            //当前节点的右上格最小和
            if (i > 0 && j < m - 1) {
                dp[i][j] = Math.min(dp[i - 1][j + 1] + A[i][j], dp[i][j]);
            }

        }
    }
    //最后一行里面找最小和
    for (let i = 0; i < m; i++) {
        res = Math.min(res, dp[n - 1][i]);
    }
    return res;
};
```



#### 221. 最大正方形（重做：1，*）

在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。

![image-20201218200240322](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201218200240322.png)

​																			        示例1

![image-20201218200324855](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201218200324855.png)

​																				   示例2

```mathematica
示例1：
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4

示例2：
输入：matrix = [["0","1"],["1","0"]]
输出：1

示例 3：
输入：matrix = [["0"]]
输出：0
```



```js
解法一：动态规划。
状态：dp[i,j] 表示以 (i, j) 为右下角，且只包含 1 的正方形的边长最大值

转移方程： dp(i,j)=0 （当matrix[i][j] ==0时）
         dp(i,j)=min(dp(i−1,j),dp(i−1,j−1),dp(i,j−1))+1  （当matrix[i][j]==1时）
         
结果    max = Math.max(max, dp[i][j])
/**
 * @param {character[][]} matrix
 * @return {number}
 */

var maximalSquare = function (matrix) {
    if (!matrix.length) {
        return 0;
    }

    const row = matrix.length, col = matrix[0].length;
    const dp = new Array(row).fill(0).map(_ => new Array(col));
    let max = 0;

    for(let i = 0; i < row; ++i) {
        for(let j = 0; j < col; ++j) {
            if(i===0 || j===0) {
                dp[i][j] = +matrix[i][j];
            }else {
                dp[i][j] = matrix[i][j] == 1 ? Math.min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1]) + 1 : 0;
            }
            max = Math.max(max, dp[i][j])
        }
    }
    return max * max;
};
```



### 序列型

![image-20201106164004754](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201106164004754.png)

![image-20201106164103482](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201106164103482.png)

#### 53. 最大子数组和（重做：1）

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```mathematica
示例:

输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

![image-20201008142413665](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201008142413665.png)

```javascript
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

解法三：滚动数组的思想。其实解法一就是那种意思了
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    const n = nums.length;
    const dp = [];
    let old = 0;
    let now = nums[0];
    let res = now;
    for (let i = 1; i < n; i++) {
        old = now;
        now = Math.max(old + nums[i], nums[i]);
        res = Math.max(now, res);
    }

    return res;
};
```



#### 面试题 17.16. 按摩师

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。

注意：本题相对原题稍作改动

```mathematica
示例 1：

输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
示例 2：

输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。
示例 3：

输入： [2,1,4,5,3,1,1,3]
输出： 12
解释： 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。
```

```javascript
解法一：动态规划。本质上在解决 对于第[i] 个人，我接待还是不接待的问题。判断的标准就是总价值哪个更大， 
1.那么对于接待的话就是： 当前接待的的价值 + dp[i - 2]。
2.如果不接待的话，就是： dp[i - 1]。
初始条件
  dp[0] = 0;//没有预约，0时长
  dp[1] = nums[0];//只有一个预约
状态转移方程：dp[i] = Math.max(dp[i - 2] + nums[i - 1], dp[i - 1]);

注：这里为了方便计算，令 dp[0]和 dp[1] 都等于 0，所以 dp[i] 对应的是 nums[i - 2]。

/**
 * @param {number[]} nums
 * @return {number}
 */
var massage = function (nums) {
  const dp = [];
  const n =nums.length;

  dp[0] = 0;//没有预约，0时长
  dp[1] = nums[0];//只有一个预约
  for (let i = 2; i <=n; i++){
    dp[i] = Math.max(dp[i - 2] + nums[i-1], dp[i - 1]);
  }
  return dp[n];
};
```



#### 198. 打家劫舍（重做：1）

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```mathematica
示例 1：

输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：

输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
 

提示：

0 <= nums.length <= 100
0 <= nums[i] <= 400

```

```javascript
解题思路：同按摩师那天，换了个皮而已。
/*
 * @Author: 氢氟酸hefan
 * @Date: 2019-08-27 10:48:26
 * @LastEditTime: 2020-11-02 22:43:02
 * @LastEditors: 氢氟酸hefan
 * @Description: 
 * @FilePath: \yunniubaoc:\Users\缘分tbb&hf\Desktop\test\test.js
 */
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
  const dp = [];
  const n =nums.length;

  dp[0] = 0;//没有房子，偷0枚金币
  dp[1] = nums[0];//只有一个房子
  for (let i = 2; i <=n; i++){
    dp[i] = Math.max(dp[i - 2] + nums[i-1], dp[i - 1]);
  }
  return dp[n];
};

解法二：滚动数组思想优化动态规划的空间。//注意n为0,1时的处理
/**
 * @param {number[]} nums
 * @return {number}
 */
var massage = function (nums) {
  const n = nums.length;

  if(n==0){
      return 0;
  }

  if(n==1){
      return nums[0];
  }
  let old = 0; //dp[0]
  let now = nums[0]; //dp[1]

  for (let i = 2; i <= n; i++) {
    //now = dp[i-1]
    //old = dp[i-2]
    let t= Math.max(old + nums[i - 1], now);
    old = now;
    now = t;
  }
  return now;
};
```



#### 213. 打家劫舍 II（重做：1）

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，能够偷窃到的最高金额。

```mathematica
示例 1：

输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
示例 2：

输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 3：

输入：nums = [0]
输出：0
 

提示：

1 <= nums.length <= 100
0 <= nums[i] <= 1000

```

```javascript
解法一：其实就是把环断开了，分两种情况
1.不偷第n-1个房子(nums[n-1])
2.不偷第0个房子(nums[0])
然后比较两个的最大值
比如
/**
 * @param {number[]} nums
 * @return {number}
 */
//将计算不相连时的情况封装成一个计算过程
function calc(arr) {
    const dp = [];
    const n = arr.length;

    dp[0] = 0;//没有房子，偷0枚金币
    dp[1] = arr[0];//只有一个房子
    for (let i = 2; i <= n; i++) {
        dp[i] = Math.max(dp[i - 2] + arr[i - 1], dp[i - 1]);
    }
    return dp[n];
}

var rob = function (nums) {
    const n = nums.length;
    if (n == 0) {
        return 0;
    }
    if (n == 1) {
        return nums[0];
    }

    const A = [];
    //不偷第n-1个房子(nums[n-1])
    for (let i = 0; i < n - 1; i++) {
        A[i] = nums[i];
    }
    let res1 = calc(A);

    //不偷第0个房子(nums[0])
    for (let i = 0; i < n - 1; i++) {
        A[i] = nums[i + 1];
    }
    let res2 = calc(A);

    return Math.max(res1, res2);
};
```



#### 746. 使用最小花费爬楼梯（重做：1）

数组的每个索引作为一个阶梯，第 i个阶梯对应着一个非负数的体力花费值 cost[i](索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

```mathematica
示例 1:

输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
 示例 2:

输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
注意：

cost 的长度将会在 [2, 1000]。
每一个 cost[i] 将会是一个Integer类型，范围为 [0, 999]。
```

```javascript
解法一：动态规划。
唯一不同的情况是最后一阶的台阶的花费值是否被算在内
那么我统一加最后一阶台阶，只在最后取结果的时候
比较判断去掉最后一个台阶的走法情况是否花费更少即可

/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function (cost) {
  const n = cost.length;
  const dp = [];
  dp[0] = cost[0];
  dp[1] = cost[1];
  for (let i = 2; i < n; i++) {
    dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
  }
  //比较判断去掉最后一个台阶的走法情况是否花费更少即可
  return dp[n - 2] > dp[n - 1] ? dp[n - 1] : dp[n - 2];
};
```



#### 322. 零钱兑换（重做：1）

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。

```mathematica
示例 1：

输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
示例 2：

输入：coins = [2], amount = 3
输出：-1
示例 3：

输入：coins = [1], amount = 0
输出：0
示例 4：

输入：coins = [1], amount = 1
输出：1
示例 5：

输入：coins = [1], amount = 2
输出：2
 

提示：

1 <= coins.length <= 12
1 <= coins[i] <= 231 - 1
0 <= amount <= 104


```

```javascript
解法一：动态规划。
状态：dp[i] = 最少用多少枚硬币拼出i元

若硬币只有2,5,7的面额
转移方程： dp[i] = Math.min((dp[i-2]+1,dp[i-5]+1,dp[i-7]+1))//写代码时都得注意下标限制

通用表达转移方程为： dp[i] = Math.min(dp[i - coins[j]] + 1);

初始条件：dp[0] = 0;0元只要0枚硬币

答案：dp[amount] === Infinity ? -1 : dp[amount];

/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function (coins, amount) {
    const dp = [];
    dp[0] = 0;
    const n = coins.length;

    for (let i = 1; i <= amount; i++) {
        dp[i] = Infinity;
        for (let j = 0; j < n; j++) {
            if (i >= coins[j] && dp[i - coins[j]] != Infinity) {
                dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);
            }
        }
    }

    return dp[amount] === Infinity ? -1 : dp[amount];
};
```



#### 70. 爬楼梯（重做:1）

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

```mathematica
示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶


```

```js
解法一：动态规划。实际就是斐波拉契数列。

如果第一次只跳一级，则后面剩下的n-1级台阶的跳法数目为f(n-1)；如果第一次跳两级，则后面剩下的n-2级台阶的跳法数目为f(n-2)。所以，得出递归方程，f(n) = f(n-1) + f(n-2)。问题本质是斐波那契数列。

状态：dp[i]为爬到i级楼梯时最多有多少种方法，那么dp[i-1]和dp[i-2]同样代表爬到i-1和i-2级楼梯时最多有多少种方法.

转移方程：dp[i] = dp[i - 1] + dp[i - 2];爬到i级楼梯的方法等于前两个楼梯爬上来之和

初始条件：    dp[0] = 1; //这个初始条件也需要举个dp[2]的例子可以验证下，或者认为（爬0层台阶有一种方法，不爬就是楼顶）dp[1] = 1;//爬到第一层时只有一种方法上来

/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
    const dp = [];
    dp[0] = 1; //这个初始条件也需要举个dp[2]的例子可以验证下，或者认为（爬0层台阶有一种方法，不爬就是楼顶）
    dp[1] = 1;
    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
};



//滚动数组：
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
    let old = 1;//old 替换dp[0]
    let now = 1;//now 替换dp[1]
    for (let i = 2; i <= n; i++) {
        if (i >= 2) {
            let t = now + old;
            old = now;
            now = t;
        }
    }
    return now;
};
```



#### 122. 买卖股票的最佳时机 II（重做：1）

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

```mathematica
示例 1:

输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
 

提示：

1 <= prices.length <= 3 * 10 ^ 4
0 <= prices[i] <= 10 ^ 4

```

![image-20201107214847056](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107214847056.png)

```javascript
解法一：这题还不算是动态规划。关键是理解题意。
这题能任意的交易，多次买卖股票。那么想要利润最大，想象成有一副股票趋势图，那么我们只需要图里面每段上升的区间作为利润，那么利润之和肯定是最大。
倒是有点贪心的思想，局部最优。

/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
    const n = prices.length;
    if (n == 0) {
        return 0;
    }
    let sum = 0;
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            sum += prices[i] - prices[i - 1];
        }
    }
    return sum;
};
```

#### 123. 买卖股票的最佳时机 III（重做：1，*）

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

**注意:** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

```mathematica
示例 1:

输入: [3,3,5,0,0,3,1,4]
输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:

输入: [7,6,4,3,1] 
输出: 0 
解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。


```

![image-20201107224857287](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107224857287.png)

![image-20201107225014083](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107225014083.png)

```javascript
解法一：动态规划。按照第一次买，第一次卖，第二次买，第二次卖4个划分，划分成5个阶段
结果：因为最多买卖两次，所以答案是max{f[N][1], f[N][3], f[N][5]}, 即清仓状态下最后一天最大获利

/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {

    // 1 2 3 4 5  五个阶段
    // 2 4 为持有股票时期
    // 1 3 5 为非持有股票时期
    //prices数组是从0开始的，所以会和dp差一个下标
    
    const n = prices.length;
    const dp = [];
    for (let i = 0; i <= n; i++) {
        dp[i] = [];
    }
    dp[0][1] = 0;///前0天处于阶段1
    for (let i = 2; i <= 5; i++) {
        dp[0][i] = -Infinity; //前0天后面的阶段看做负无穷，因为后面求的是最大利润
    }

    for (let i = 1; i <= n; i++) {
        for (let k = 1; k <= 5; k += 2) {
            //1 3 5 阶段，
            //dp[i - 1][k]昨天非持有股票，
            //dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2]昨天持有股票，今天卖出清仓
            dp[i][k] = dp[i - 1][k];
            if (k > 1 && i >= 2 && dp[i - 1][k - 1] != -Infinity) {
                dp[i][k] = Math.max(dp[i][k], dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2])
            }
        }

        for (let k = 2; k <= 5; k += 2) {
            //2 4 阶段，
            //dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2]昨天持有股票，今天继续持有并获利
            //dp[i - 1][k-1]昨天非持有股票，今天买入
            dp[i][k] = dp[i - 1][k - 1];
            if (i >= 2 && dp[i - 1][k] != -Infinity) {
                dp[i][k] = Math.max(dp[i][k], dp[i - 1][k] + prices[i - 1] - prices[i - 2])
            }
        }
    }
    return Math.max(dp[n][1], dp[n][3], dp[n][5]);
};
```



#### 188. 买卖股票的最佳时机 IV（重做：1，*）

给定一个整数数组 prices ，它的第 i 个元素 prices[i] 是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

**注意:** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

```mathematica
示例 1：

输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
示例 2：

输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
 

提示：

0 <= k <= 109
0 <= prices.length <= 104
0 <= prices[i] <= 1000

```

![image-20201107230231948](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107230231948.png)

```js
解法一：动态规划。其实和上题差不多了，只不过阶段扩展到了2K+1。按照第一次买，第一次卖，第二次买，第二次卖....第K次买，第K次卖划分。划分成个2K+1阶段。

/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (K, prices) {

    // 1 2 3 4 5... 2k+1 个阶段
    // 2 4..2k 为持有股票时期
    // 1 3 5...2k+1 为非持有股票时期
    //prices数组是从0开始的，所以会和dp差一个下标

    const n = prices.length;
    const dp = [];
    for (let i = 0; i <= n; i++) {
        dp[i] = [];
    }
    dp[0][1] = 0;///前0天处于阶段1
    for (let i = 2; i <= 2 * K + 1; i++) {
        dp[0][i] = -Infinity; //前0天后面的阶段看做负无穷，因为后面求的是最大利润
    }

    for (let i = 1; i <= n; i++) {
        for (let k = 1; k <= 2 * K + 1; k += 2) {
            // 1 2 3 4 5... 2k+1 个阶段，
            //dp[i - 1][k]昨天非持有股票，
            //dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2]昨天持有股票，今天卖出清仓
            dp[i][k] = dp[i - 1][k];
            if (k > 1 && i >= 2 && dp[i - 1][k - 1] != -Infinity) {
                dp[i][k] = Math.max(dp[i][k], dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2])
            }
        }

        for (let k = 2; k <= 2 * K; k += 2) {
            // 2 4..2k 阶段，
            //dp[i - 1][k - 1] + prices[i - 1] - prices[i - 2]昨天持有股票，今天继续持有并获利
            //dp[i - 1][k-1]昨天非持有股票，今天买入
            dp[i][k] = dp[i - 1][k - 1];
            if (i >= 2 && dp[i - 1][k] != -Infinity) {
                dp[i][k] = Math.max(dp[i][k], dp[i - 1][k] + prices[i - 1] - prices[i - 2])
            }
        }
    }
    let res = -Infinity;
    for (let i = 1; i <= 2 * K + 1; i += 2) {
        res = Math.max(res, dp[n][i]);
    }
    return res;
};
```



#### 309. 最佳买卖股票时机含冷冻期

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

```mathematica
示例:

输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

```javascript
解法一：动态规划。
和上题股票4有点类似。
最后一步就是在前n天的收益是多少

状态：设dp[i][k]为前i天的最大利润，k为两个阶段，一个是持有股票0,一个是没持有股票1

转移方程：
持有股票时，分为 
	昨天就持有股票，今天休息dp[i - 1][0]。
	前天卖了股票，今天买了股票dp[i - 2][1] - prices[i - 1]。
    dp[i][0] = Math.max(dp[i - 1][0], dp[i - 2][1] - prices[i - 1]);
未持有股票时，分为
	昨天也没有持有，今天也没有持有dp[i-1][1]
	昨天持有股票，今天卖了股票dp[i - 1][0] + prices[i - 1]
	dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i - 1]);
	
初始条件：在这题中，初始条件可能容易弄混，一定得弄清楚dp的下标，i等于0时就是第0天，而prices[0]已经是第一天了。
dp[i][0] = -prices[0];//第1天如果持股的话，则收益-prices[0];
dp[i][1] = 0;//第1天如果没持股的话，则收益0;

结果:dp[n][1] 第n天不持股的时候

/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
    //k 0 1 2
    //0持股
    //1没持股
    const n = prices.length;
    if (n == 0) {
        return 0;
    }
    const dp = [];
    for (let i = 0; i <= n; i++) {
        dp[i] = [];
    }
    dp[0][1] = 0;//第0天收益0
    dp[0][0] = 0;//第0天收益0
    for (let i = 1; i <= n; i++) {
        for (let k = 0; k < 2; k++) {
            if (k == 0) {
                if (i == 1) {
                    dp[i][0] = -prices[0];//第1天如果持股的话，则收益-prices[0];
                    continue;
                }
                //昨天就持有股票，今天休息。
                dp[i][0] = dp[i - 1][0];
                if (i >= 2) {
                    //前天卖了股票，今天买了股票。
                    dp[i][0] = Math.max(dp[i][0], dp[i - 2][1] - prices[i - 1]);
                }
            }
            if (k == 1) {
                //dp[i-1][1]昨天也没有持有，今天也没有持有。dp[i - 1][0] + prices[i - 1]昨天持有股票，今天卖了股票。
                if (i == 1) {
                    dp[i][1] = 0;//第1天如果没持股的话，则收益0;
                    continue;
                }

                dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i - 1]);
            }
        }
    }

    return dp[n][1];
}   

```



#### 354. 俄罗斯套娃信封问题(重做：1)

给定一些标记了宽度和高度的信封，宽度和高度以整数对形式 (w, h) 出现。当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算最多能有多少个信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

说明:
不允许旋转信封。

```mathematica
示例:

输入: envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出: 3 
解释: 最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。

```

```javascript
解法一：动态规划。
将信封先按宽度排序。
状态：设dp[i]表示以E为最外层信封时最多的嵌套层数

转移方程：dp[i] = Math.max(dp[j] + 1, 1);（Ej能够存放在Ei里面）

初始条件：无

结果：Math.max(dp[0],dp[1]....dp[n-1]);

/**
 * @param {number[][]} envelopes
 * @return {number}
 */
var maxEnvelopes = function (envelopes) {
    envelopes.sort((v1, v2) => {
        if (v1[0] != v2[0]) {
            return v1[0] - v2[0];
        } else {
            return v1[1] - v2[1];
        }
    })

    const n = envelopes.length;
    if (n == 0) {
        return 0;
    }

    const dp = [];
    let res = 0;
    for (let i = 0; i < n; i++) {
        dp[i] = 1;
        for (let j = 0; j < i; j++) {
            if (envelopes[i][0] > envelopes[j][0] && envelopes[i][1] > envelopes[j][1]) {
                dp[i] = Math.max(dp[j] + 1, dp[i]);
            }
        }
        res = Math.max(res, dp[i]);
    }
    return res;
};

```



### 划分型

![image-20201108224053252](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201108224053252.png)

![image-20201107163729388](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107163729388.png)

#### 279. 完全平方数（重做：1）

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。

```mathematica
示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.


```

```javascript
解法一：动态规划。
最后一步：关注最优策略中最后一个完全平方数 j^2
那么n-j^2也一定被划分成最少完全平方数之和

状态：设dp[i]为表示i最少被分成几个完全平方数之和

转移方程：dp[i] = Math.min( dp[i - j * j] + 1);(1<=j*j=<i)

初始条件：dp[0]=0,(0被分成0个完全平方数之和，因为0不是完全平方数。或者用dp[1]=1验证下)

答案：dp[n]

/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function (n) {
    const dp = [];
    dp[0] = 0;
    for (let i = 1; i <= n; i++) {
        dp[i] = Infinity;
        for (let j = 1; j * j <= i; j++) {
            if (dp[i - j * j] != Infinity) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
    }
    return dp[n];
};
```



#### 132. 分割回文串 II

给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回符合要求的最少分割次数。

```mathematica
示例:

输入: "aab"
输出: 1
解释: 进行一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

```javascript
解法一：动态规划。
状态：设dp[i]为s前i个字符s[0...i-1]最少可以划分几个回文串。

转移方程：dp[i] = Math.min(dp[i], dp[j] + 1);(s[j....i-1]为回文串)

初始条件:dp[0]=0;(空串可以被分成0个回文串,用dp[1]测试下便知)

结果：dp[n] -1;(原题要求是求划分次数)

/**
 * @param {string} s
 * @return {number}
 */
function calc(S) {
    const n = S.length;
    const SisPlalindrome = [];
    for (let i = 0; i < n; i++) {
        SisPlalindrome[i] = [];
    }
    let c, i, j;
    for (i = 0; i < n; i++) {
        for (j = i; j < n; j++) {
            SisPlalindrome[i][j] = false;
        }
    }

    // if (n % 2 == 1) {
    //     //奇数情况，中间的字符为界限,用i，j向两边扩展
    //     for (c = 0; c < n; c++) {
    //         i = j = c;
    //         console.log(i, j);
    //         while (i >= 0 && j < n && S[i] == S[j]) {
    //             SisPlalindrome[i][j] = true;
    //             i--;
    //             j++;
    //             console.log(i, j);
    //         }
    //     }
    // } else {
    //     //偶数情况，两个字符中间的线为界限,用i，j向两边扩展,注意j+c+1,则c<n-1为判断条件
    //     for (c = 0; c < n - 1; c++) {
    //         i = c;
    //         j = c + 1;
    //         while (i >= 0 && j < n && S[i] == S[j]) {
    //             SisPlalindrome[i][j] = true;
    //             i--;
    //             j++;
    //         }
    //     }
    // }


    //这里不能分情况用if else隔开奇偶数，因为如果当"aab"测试时，可以看到奇数情况代码里面，是无法判断SisPlalindrome[0][1]的情况的
    //奇数情况，中间的字符为界限,用i，j向两边扩展
    for (c = 0; c < n; c++) {
        i = j = c;
        while (i >= 0 && j < n && S[i] == S[j]) {
            SisPlalindrome[i][j] = true;
            i--;  //向左边扩展
            j++;  //向右边扩展
        }
    }

    //偶数情况，两个字符中间的线为界限,用i，j向两边扩展
    for (c = 0; c < n-1; c++) {
        i = c;
        j = c + 1;
        while (i >= 0 && j < n && S[i] == S[j]) {
            SisPlalindrome[i][j] = true;
            i--;
            j++;
        }
    }

    return SisPlalindrome;
}
var minCut = function (s) {
    //dp[i] = dp[i - j] + 1;//(s[0,j-1] ,s[j,i-1];)
    const n = s.length;
    if (n == 0) {
        return 0;
    }
    const dp = [];
    dp[0] = 0;

    let SisPlalindrome = calc(s);
    for (let i = 1; i <= n; i++) {
        dp[i] = Infinity;
        for (let j = 0; j < i; j++) {
            //这里判断s[j...i-1]是不是回文串，用了点操作，将O(n^3)的复杂度降到了O(n^2)
            if (SisPlalindrome[j][i - 1]) {
                dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
    }

    return dp[n] - 1;
};
```



#### 5. 最长回文子串（重做：1）

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

```mathematica
示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"
```

```javascript
解法一：双指针滑动窗口。（中心扩展法）

/**
 * @param {string} s
 * @return {string}
 */


var longestPalindrome = function (s) {
    let start = 0, maxLen = 0;
    let c, i, j;
    const n = s.length;
    //奇数情况，中间的字符为界限,用i，j向两边扩展
    for (let c = 0; c < n; c++) {
        i = j = c;
        while (i >= 0 && j < n && s[i] == s[j]) {
            if (j - i + 1 > maxLen) {
                maxLen = j - i + 1;
                start = i;
            }
            i--;
            j++;
        }
    }
     //偶数情况，两个字符中间的线为界限,用i，j向两边扩展
    for (let c = 0; c < n - 1; c++) {
        i = c;
        j = c + 1;
        while (i >= 0 && j < n && s[i] == s[j]) {
            if (j - i + 1 > maxLen) {
                maxLen = j - i + 1;
                start = i;
            }
            i--;
            j++;
        }
    }
    return s.slice(start, maxLen + start);
};

```



#### 647. 回文子串（重做：1）

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

```mathematica
示例 1：

输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
示例 2：

输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
 

提示：

输入的字符串长度不会超过 1000 。

```

```javascript
解法一：双指针滑动窗口。

/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function (s) {
    const n = s.length;
    const SisPlalindrome = [];
    for (let i = 0; i < n; i++) {
        SisPlalindrome[i] = [];
    }

    let c, i, j;

    //奇数情况
    for (c = 0; c < n; c++) {
        i = j = c;
        while (i >= 0 && j < n && s[i] == s[j]) {
            SisPlalindrome[i][j] = true;
            i--;
            j++;
        }
    }

    //偶数情况
    for (c = 0; c < n - 1; c++) {
        i = c;
        j = c + 1;
        while (i >= 0 && j < n && s[i] == s[j]) {
            SisPlalindrome[i][j] = true;
            i--;
            j++;
        }
    }

    let res = 0;
    for (i = 0; i < n; i++) {
        for (j = i; j < n; j++) {
            if (SisPlalindrome[i][j] == true) {
                res++;
            }
        }
    }

    return res;
};
```



#### 312. 戳气球

有 n 个气球，编号为0 到 n-1，每个气球上都标有一个数字，这些数字存在数组 nums 中。

现在要求你戳破所有的气球。如果你戳破气球 i ，就可以获得 nums[left] * nums[i] * nums[right] 个硬币。 这里的 left 和 right 代表和 i 相邻的两个气球的序号。注意当你戳破了气球 i 后，气球 left 和气球 right 就变成了相邻的气球。

求所能获得硬币的最大数量。

```mathematica
说明:

你可以假设 nums[-1] = nums[n] = 1，但注意它们不是真实存在的所以并不能被戳破。
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100
示例:

输入: [3,1,5,8]
输出: 167 
解释: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
     coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167


```

```javascript
解法一：动态规划。
状态：我们假设最后戳破的气球是 k，戳破 k 获得最大数量的银币就是 nums[i] * nums[k] * nums[j] 再加上前面戳破的最大数量和后面的最大数量，即：nums[i] * nums[k] * nums[j] + 前面最大数量 + 后面最大数量
（注意 i 不一定是 k - 1，同理 j 也不一定是 k + 1，因此可能 i - 1 和 i + 1 已经被戳破了。）
dp[i][j] = x 表示戳破气球 i 和气球 j 之间（开区间，不包括 i 和 j）的所有气球，可以获得的最大数量为 x

转移方程：dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + nums[k] * nums[i] * nums[j])。

初始条件：由于我们利用了两个虚拟气球，边界就是气球数 n + 2
初始值，当 i == j 时，很明显两个之间没有气球，所有为 0；

条件：dp[0][n + 1]
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxCoins = function (nums) {
    const n = nums.length;
    const points = [1, ...nums, 1];
    //dp[i][j] = x 表示戳破气球 i 和气球 j 之间（开区间，不包括 i 和 j）的所有气球
    const dp = Array.from(Array(n + 2), () => Array(n + 2).fill(0));

    //假设最后戳破的气球是 k，戳破 k 获得最大数量的银币就是 nums[i] * nums[k] * nums[j] 再加上前面戳破的最大数量和后面的最大数量，即：nums[i] * nums[k] * nums[j] + 前面最大数量 + 后面最大数量

    for (let i = n; i >= 0; i--) {
        for (let j = i + 1; j <= n + 1; j++) {
            for (let k = i + 1; k < j; k++) {
                dp[i][j] = Math.max(dp[i][j], points[i] * points[k] * points[j] + dp[i][k] + dp[k][j]);
            }
        }
    }
    return dp[0][n + 1];
};
```



### 博弈型

![image-20201109142047265](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201109142047265.png)

![image-20201109142059036](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201109142059036.png)

### 背包型

![image-20201109142657401](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201109142657401.png)

![image-20201107184440036](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201107184440036.png)

### 位操作型

####  338. 比特位计数（重做：1）

给定一个非负整数 **num**。对于 **0 ≤ i ≤ num** 范围中的每个数字 **i** ，计算其二进制数中的 1 的数目并将它们作为数组返回。

```mathematica
示例 1:

输入: 2
输出: [0,1,1]
示例 2:

输入: 5
输出: [0,1,1,2,1,2]
进阶:

给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
要求算法的空间复杂度为O(n)。
你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。

```



```javascript
解法一：动态规划。
状态：dp[i]为求i的二进制表示中有多少1，则在i的二进制去掉最后一位i mod 2，设新的数是Y=(i>>1) (右移一位)

转移方程：dp[i]= dp[i>>1] + (i mod 2);

初始条件： dp[0]=0;

结果：dp数组

/**
 * @param {number} num
 * @return {number[]}
 */
var countBits = function (num) {

    const dp = [];
    dp[0] = 0;
    for (let i = 1; i <= num; i++) {
        //dp[i]= dp[i>>1] + (i % 2);
        dp[i] = dp[i >> 1] + (i & 1); //这样子i&1就相当于模2的作用了，速度更快
    }
    return dp;
};
```



## 位运算

### 461. 汉明距离

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

注意：
0 ≤ x, y < 2^31.

```mathematica
示例:

输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

上面的箭头指出了对应二进制位不同的位置。

```

```javascript
解法一：位运算+与运算。
从最后一位比较，依次向右移位，最后一位不同则计数。

/**
 * @param {number} x
 * @param {number} y
 * @return {number}
 */
var hammingDistance = function (x, y) {
  let count = 0;
  while (x != 0 || y != 0) {
    if ((x & 1) !== (y & 1)) {//按位与运算符 (&) 在每个位上返回 1 ，这两个操作数对应的位都是 1。实际在这就是比较最后一位是1还是0
      count++;
    }
    x >>= 1;
    y >>= 1;
  }
  return count;
};

```



## 桶思想

### 621. 任务调度器

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个相同种类的任务之间必须有长度为 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的最短时间。

```mathematica
示例 ：

输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 

提示：

任务的总个数为 [1, 10000]。
n 的取值范围为 [0, 100]。
```

![image-20201031211937245](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201031211937245.png)

建立大小为n+1的桶子，个数为任务数量最多的那个任务，比如下图，等待时间n=2，A任务个数6个，我们建立6个桶子，每个容量为3

![image-20201031212032864](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201031212032864.png)

![image-20201031212110406](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201031212110406.png)



```javascript
解法一：桶思想。
1.记录最大任务数量N，看一下任务数量并列最多的任务有多少个，即最后一个桶子的任务数X，计算NUM1=（N-1）*（n+1）+x
2.NUM2=tasks.size()

/**
 * @param {character[]} tasks
 * @param {number} n
 * @return {number}
 */
var leastInterval = function (tasks, n) {
  let myBucket = {};
  let maxBucket = -Infinity;
  for (let i = 0; i < tasks.length; i++) {
    if (myBucket[tasks[i]]) {
      myBucket[tasks[i]]++;
      maxBucket = Math.max(maxBucket, myBucket[tasks[i]]);
    } else {
      myBucket[tasks[i]] = 1;
    }
  }
  let num1 = (maxBucket - 1) * (n + 1);
  let num2 = tasks.length;
  let myBucketKeys = Object.keys(myBucket);
  for (let j = 0; j < myBucketKeys.length; j++) {
    if (myBucket[myBucketKeys[j]] === maxBucket) {
      num1++;//最后一桶的任务数
    }
  }
  return Math.max(num1, num2);
};
```

### 347. 前 K 个高频元素（重做：1，*）

给定一个非空的整数数组，返回其中出现频率前 **k** 高的元素。

```mathematica
示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]
 

提示：

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。

```

```js
解法一:桶排序。这个桶是一个二维数组
用一个哈希表，记录每个值和次数
用一个桶bucket，下标存频率次数，值是每个频率下的各个元素，下标为6的话，值为['1','2']的话，说明1,2这两个数出现了6次
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function (nums, k) {
    const len = nums.length;
    const valTimesMap = {}, result = [];
    for (let num of nums) {
        valTimesMap[num] ? valTimesMap[num]++ : valTimesMap[num] = 1;
    }

    //用一个桶，下标存频率次数，值是每个频率下的各个元素，下标为6的话，值为['1','2']的话，说明1,2这两个数出现了6次
    const bucket = new Array(len+1).fill(0).map(_ => []);
    for (let key of Object.keys(valTimesMap)) {
        bucket[valTimesMap[key]].push(+key);
    }
    for (let i = bucket.length-1 ; i >= 1 && k > 0; --i) {
        result.push(...bucket[i])
        k -= bucket[i].length;
    }
    return result;

    // O(n) 与 O(n log n) 
    // const numSet = [...new Set(nums)];
    // numSet.sort((a, b) => valTimesMap.get(b) - valTimesMap.get(a));
    // return numSet.slice(0, k); 
};
```



## 排序

### 快排

![image-20201109190720655](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201109190720655.png)



![image-20201201140846077](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201201140846077.png)

#### 973.最接近原点的 K 个点

我们有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

```mathematica
示例 1：

输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
示例 2：

输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
 

提示：

1 <= K <= points.length <= 10000
-10000 < points[i][0] < 10000
-10000 < points[i][1] < 10000

```

```js

```



#### 215. 数组中的第K个最大元素（重做：1，*）

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

```mathematica
示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4


```

```javascript
解法一：快排

/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function (nums, k) {
    
    const quickSort = (left, right) => {
        let i = left, j = right;
        if (i < j) {
            const temp = nums[i];
            while (i < j) {
                while (i < j && nums[j] > temp) j--;
                if (i < j) {
                    nums[i] = nums[j];
                    i++;
                }
                while (i < j && nums[i] < temp) i++;
                if (i < j) {
                    nums[j] = nums[i];
                    j--;
                }
            }
            nums[i] = temp;
            quickSort(left, i - 1);//对左区间递归排序
            quickSort(i + 1, right);//对右区间递归排序
        }
    }
    
    quickSort(0, nums.length - 1);
    return nums[nums.length - k];
};

解法二：
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function(nums, k) {
    const len = nums.length;

    const partition = (left, right) => {
        let pivot = nums[left];

        while(left < right) {
            while(left < right && nums[right] >= pivot) {
                right--;
            }
            left < right && (nums[left++] = nums[right]);

            while(left < right && nums[left] <= pivot) {
                left++;
            }
            left < right && (nums[right--] = nums[left]);
        }

        nums[left] = pivot;
        return left;
    }

    const findKthSmall = (left, right, kSmall) => {
        if(left === right) {
            return nums[left];
        }

        const pivot = partition(left, right);
        if(pivot === kSmall - 1) {
            return nums[pivot];
        } else if(pivot < kSmall - 1) {
            return findKthSmall(pivot + 1, right, kSmall);
        } else {
            return findKthSmall(left, pivot - 1, kSmall);
        }
    }

    return findKthSmall(0, len - 1, len - k + 1);
};
```



#### 148. 排序链表（*）

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

**进阶：**

你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

![image-20201116212445952](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201116212445952.png)

![image-20201116212505835](C:\Users\缘分tbb&hf\AppData\Roaming\Typora\typora-user-images\image-20201116212505835.png)

```mathematica
提示：

链表中节点的数目在范围 [0, 5 * 104] 内
-105 <= Node.val <= 105
```

```javascript
解法一：
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

// 快排实现，以后写

//归并排序，找中点(快慢指针/记录长度 本质复杂度一样)
var sortList = function(head) {
    if(!head || !head.next) {
        return head;
    }

    const mid = findMiddleNode(head);

    const right = sortList(mid.next);
    mid.next = null;
    const left = sortList(head);

    return merge(left, right);
};  

var merge = function(l1, l2) {
    if(l1 === null){
        return l2;
    }
    if(l2 === null){
        return l1;
    }
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else{
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
function findMiddleNode(head) {
    let slow = head, fast = head.next;
    while(fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

解法二：用数组代替了一下。
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var sortList = function (head) {
    let res = [];
    let p = head, temp = head;
    while (head) {
        res.push(head.val);
        head = head.next;
    }
    res.sort((v1, v2) => v1 - v2);
    for (let i = 0; i < res.length; i++) {
        p.val = res[i];
        p = p.next;
    }
    return temp;
};
```

