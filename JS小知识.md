# JS小知识

### 0. 首思考

- 优先使用...
- return 后常用 三目运算符
- event.preventDefault && event.preventDefault()  如果有 event.preventDefault 就用 event.preventDefault()





### 1. parseInt()

parseInt（）的第二个参数指定要解析的类型，最终的结果永远都是10进制的结果。

num.toString(16) 返回16进制的字符串



### 2.数字

在内部，数字是以 64 位格式 [IEEE-754](http://en.wikipedia.org/wiki/IEEE_754-1985) 表示的，所以正好有 64 位可以存储一个数字：其中 52 位被用于存储这些数字，其中 11 位用于存储小数点的位置（对于整数，它们为零），而 1 位用于符号。 01010101010

alert( 0.1 + 0.2 ); // 0.30000000000000004

请注意，`63.5` 完全没有精度损失。这是因为小数部分 `0.5` 实际上是 `1/2`。以 2 的整数次幂为分母的小数在二进制数字系统中可以被精确地表示，现在我们可以对它进行舍入





### 3. 数组的使用

Javascript 引擎会发现，我们在像使用常规对象一样使用数组，那么针对数组的优化就不再适用了，然后对应的优化就会被关闭，这些优化所带来的优势也就荡然无存了。

数组误用的几种方式:

- 添加一个非数字的属性，比如 `arr.test = 5`。
- 制造空洞，比如：添加 `arr[0]`，然后添加 `arr[1000]` (它们中间什么都没有)。
- 以倒序填充数组，比如 `arr[1000]`，`arr[999]` 等等。





### 4. 迭代器

为了让 `range` 对象可迭代（也就让 `for..of` 可以运行）我们需要为对象添加一个名为 `Symbol.iterator` 的方法（一个专门用于使对象可迭代的内建 symbol）。

1. 当 `for..of` 循环启动时，它会调用这个方法（如果没找到，就会报错）。这个方法必须返回一个 **迭代器（iterator）** —— 一个有 `next` 方法的对象。
2. 从此开始，`for..of` **仅适用于这个被返回的对象**。
3. 当 `for..of` 循环希望取得下一个数值，它就调用这个对象的 `next()` 方法。
4. `next()` 方法返回的结果的格式必须是 `{done: Boolean, value: any}`，当 `done=true` 时，表示循环结束，否则 `value` 是下一个值。

请注意可迭代对象的核心功能：关注点分离。

- `range` 自身没有 `next()` 方法。
- 相反，是通过调用 `range[Symbol.iterator]()` 创建了另一个对象，即所谓的“迭代器”对象，并且它的 `next` 会为迭代生成值。

因此，迭代器对象和与其进行迭代的对象是分开的。





### 5.weakMap

``` js
let john = { name: "John" };

let array = [ john ];

john = null; // 覆盖引用

// 前面由 john 所引用的那个对象被存储在了 array 中
// 所以它不会被垃圾回收机制回收
// 我们可以通过 array[0] 获取到它
```

现在，如果我们在 weakMap 中使用一个对象作为键，并且没有其他对这个对象的引用 —— 该对象将会被从内存（和map）中自动清除。

``` js
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // 覆盖引用

// john 被从内存中删除了！	
```

与上面常规的 `Map` 的例子相比，现在如果 `john` 仅仅是作为 `WeakMap` 的键而存在 —— 它将会被从 map（和内存）中自动删除。





### 6.结构赋值

**交换变量值的技巧**

有一个著名的使用结构赋值来交换两个变量的值的技巧：

``` js
let guest = "Jane";
let admin = "Pete";

// 让我们来交换变量的值：使得 guest = Pete，admin = Jane
[guest, admin] = [admin, guest];

alert(`${guest} ${admin}`); // Pete Jane（成功交换！）
```





### 7.关于this

``` html
<body>

    <button>点击1</button>
    <button>点击2</button>
    <button>点击3</button>

    <script>
        var btn = document.getElementsByTagName("button")
        for (var i=0; i < btn.length; i++) {
            btn[i].onclick = function () {
                console.log(this.innerHTML);
                console.log(btn);
                console.log(i);
                console.log(btn[i]);
            }
        }
    </script>
</body>
```

btn[i] 中的i 能够保存下来  而后面的function中的函数不行 是异步的  里面的i=3
但是this能够正确指向 因为谁调用this就是谁



### 8.对象

**js 打印这个对象的语句执行的时候, 对象里面的确是有值的,但是当程序继续执行,下面的代码有对这个对象赋值的语句,所以chrome控制台显示,里面有值。 打印时内存里的值已经变了 ，导致 展开和收起不一致。**







### 杂项

- ``` js
  if (getComputedStyle) {
      return 
  } else {
      return 
  }
  ```

  如果没有 getComputedStyle 是找不到变量 则直接报错 页面渲染失败   但是写 window.getComputedStyle 是找不到属性 不影响





### 9.逻辑运算符

``` js
s[i]==='('||s[i]==='['||s[i]==='{' // 正确的
s[i]=== '(' || '[' || '{' // 错误

//优先级问题
```

