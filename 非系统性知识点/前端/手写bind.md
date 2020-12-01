bind 用法不难，一句话解释就是把新的 this 绑定到某个函数 func 上，并返回 func 的一个拷贝。使用方法如下：
`let boundFunc = func.bind(thisArg[, arg1[, arg2[, ...argN]]])`
手写 bind 可以分为三个境界：
1. 初级：只用 ES6 新语法优点
	- 因为可以使用 const 、... 操作符，代码简洁。
	- 缺点：兼容性稍差。
2. 中级：使用 ES5 语法
	- 优点：兼容 IE（其实可以忽略）。
	- 缺点：参数要用Array.prototype.slice 获取，复杂且不支持 new。
3. 高级：ES5 + 支持 new
	- 优点：支持 new。
	- 缺点：最复杂。

## 初级 bind

这种方式的优点是因为可以使用 const 、... 操作符，代码简洁；缺点是不兼容 IE 等一些古老浏览器
```
function bind_1(asThis, ...args) {
  const fn = this; // 这里的 this 就是调用 bind 的函数 func
  return function (...args2) {
    return fn.apply(asThis, ...args, ...args2);
  };
}
```

## 中级 bind

优点：兼容 IE缺点：参数要用Array.prototype.slice 取，复杂且不支持 new
```
// 中级：兼容 ES5
function bind_2(asThis) {
  var slice = Array.prototype.slice;
  var args = slice.call(arguments, 1);
  var fn = this;
  if (typeof fn !== "function") {
    throw new Error("cannot bind non_function");
  }
  return function () {
    var args2 = slice.call(arguments, 0);
    return fn.apply(asThis, args.concat(args2));
  };
}
```

## 高级 bind
优点：支持 new缺点：最复杂



写之前，我们先来看一看我们应该如何判断 `new`，
`new fn(args)` 其实等价于：
```
const temp = {}
temp.__proto__ = fn.prototype
fn.apply(temp, [...args])
return temp
```
核心在第二句：`temp.__proto__ = fn.prototype`，有了这个，我们便知道可以用 fn.prototype 是否为对象原型来判断是否为 `new` 的情况。
```
// 高级：支持 new，例如 new (funcA.bind(thisArg, args))
function bind_3(asThis) {
  var slice = Array.prototype.slice;
  var args1 = slice.call(arguments, 1);
  var fn = this;
  if (typeof fn !== "function") {
    throw new Error("Must accept function");
  }
  function resultFn() {
    var args2 = slice.call(arguments, 0);
    return fn.apply(
      resultFn.prototype.isPrototypeOf(this) ? this : asThis, // 用来绑定 this
      args1.concat(args2)
    );
  }
  resultFn.prototype = fn.prototype;
  return resultFn;
}
```