如果要硬按照 Promises/A+ 规范来写，可能至少要 2-3 个小时，400+行代码，这种情况是几乎不可能出现在面试中。所以我们只需要完成一个差不多的版本，保留最核心的功能。

核心功能：
- `new Promise(fn)` 其中 `fn` 只能为函数，且要**立即执行**。
- `promise.then(success, fail)` 中的 `success` 是函数，且会在 `resolve` 被调用的时候执行，fail 同理实。

## 实现思路：
1. `then(succeed, fail) `先把成功失败回调放到一个回调数组 `callbacks[]` 上。
2. `resolve()` 和 `reject()` 遍历 callbacks。
3. `resolve()` 读取成功回调 / `reject()`读取失败回调，并异步执行 callbacks 里面的成功和失败回调（放到本轮的微任务队列中）。

下面提供根据上述需求及思路实现的模板：

```
作者：Venaissance
链接：https://zhuanlan.zhihu.com/p/160315811
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

class Promise2 {
  state = "pending";
  callbacks = [];
  constructor(fn) {
    if (typeof fn !== "function") {
      throw new Error("must pass function");
    }
    fn(this.resolve.bind(this), this.reject.bind(this));
  }
  resolve(result) {
    if (this.state !== "pending") return;
    this.state = "fulfilled";
    nextTick(() => {
      this.callbacks.forEach((handle) => {
        if (typeof handle[0] === "function") {
          handle[0].call(undefined, result);
        }
      });
    });
  }
  reject(reason) {
    if (this.state !== "pending") return;
    this.state = "rejected";
    nextTick(() => {
      this.callbacks.forEach((handle) => {
        if (typeof handle[1] === "function") {
          handle[1].call(undefined, reason);
        }
      });
    });
  }
  then(succeed, fail) {
    const handle = [];
    if (typeof succeed === "function") {
      handle[0] = succeed;
    }
    if (typeof fail === "function") {
      handle[1] = fail;
    }
    this.callbacks.push(handle);
  }
}

function nextTick(fn) {
  if (process !== undefined && typeof process.nextTick === "function") {
    return process.nextTick(fn);
  } else {
    // 用MutationObserver实现浏览器上的nextTick
    var counter = 1;
    var observer = new MutationObserver(fn);
    var textNode = document.createTextNode(String(counter));

    observer.observe(textNode, {
      characterData: true,
    });
    counter += 1;
    textNode.data = String(counter);
  }
}
```