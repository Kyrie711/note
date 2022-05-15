# JS 错题

### 1.this 

---



1.  ``` 
    let a = {
        name: 'john',
        ref: this
    }
    
    console.log(a.ref) // undefined 严格模式
    // 获取this 只能通过调用this中的方法
    
    let a = {
        name: 'john',
        ref: this，
        say() {
            console.log(this)
        }
    }
    ```






### 2.对象转化为原始值

``` js
var a = { value : 0 };
a[Symbol.toPrimitive] = function(hint) {
    console.log(hint); // default
    return this.value += 1;
}
console.log(a == 1 && a == 2 && a == 3); // true


var a = { value : 0 };
a.valueOf = function() {
    return this.value += 1;
};
console.log(a == 1 && a == 2 && a == 3); // true


var a = { value : 0 };
a.toString = function() {
    return this.value += 1;
};
console.log(a == 1 && a == 2 && a == 3); // true
```



### 3. Promise

``` js
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("Whoops!");
  }, 1000);
}).catch(alert);
```

throw new Error被隐式的try...catch捕获，并传入Promise的catch中。但是try...catch只能捕获同步错误，所以传不到Promise的catch中。所以捕获不到错误。reject是直接传到Promise的catch中。
