核心思路是：

- 使用一个对象作为缓存。
- on 负责把方法发布到缓存的 EventName 对应的数组。
- emit 负责遍历触发（订阅） EventName 下的方法数组。
- off 找方法的索引，并删除。

```
作者：Venaissance
链接：https://zhuanlan.zhihu.com/p/160315811
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

class EventHub {
  cache = {};
  on(eventName, fn) {
    this.cache[eventName] = this.cache[eventName] || [];
    this.cache[eventName].push(fn);
  }
  emit(eventName) {
    this.cache[eventName].forEach((fn) => fn());
  }
  off(eventName, fn) {
    const index = indexOf(this.cache[eventName], fn); // 这里用 this.cache[eventName].indexOf(fn) 完全可以，封装成函数是为了向下兼容
    if (index === -1) return;
    this.cache[eventName].splice(index, 1);
  }
}

// 兼容 IE 8 的 indexOf
function indexOf(arr, item) {
  if (arr === undefined) return -1;
  let index = -1;
  for (let i = 0; i < arr.length; ++i) {
    if (arr[i] === item) {
      index = i;
      break;
    }
  }
  return index;
}
```