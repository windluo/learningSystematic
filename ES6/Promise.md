### 基础概念

Promise 是一种解决异步编程的方案。

Promise 是一个对象，可以获取异步操作的消息。

下面是一个 Promise 实例

```js
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```



#### 特点

Promise 的状态不受外界影响，有三种状态：进行中、已成功、已失败。成功就返回`resolve()`，失败就返回`reject()`。

> 这两个都是函数，可以返回任意数据类型，也可以返回 Promise

Promise 的状态改变后不可再变。即`resolve`了或`reject`之后会一直保持这个状态，不能再改为其它状态。

#### 缺点

不能取消。一旦新建会立即执行，中途不能取消。

内部抛出的错误无法从外部得知。需要设置回调函数。

### 一些常用 API

`Promise.prototype.then`，为 Promise 实例添加状态改变时的回调函数。接收两个参数，第一个参数是`resolved`状态的回调函数，第二个参数（可选）是`rejected`状态的回调函数。

```js
promise.then(function(value){
    // resolved
}, reject(error){
	// error
})
```

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

`Promise.prototype.catch`，捕获异步执行错误的回调函数。

```js
promise.then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 promise 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

`Promise.prototype.finally`，该方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

`Promise.all()`，用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.all([p1, p2, p3]);
```

该方法需要所有的 Promise 实例结果都为 `resolved`，新实例才会返回成功。只要有一个`reject`，就会返回失败。

> 如果`.all`里的参数不是 Promise 实例，则会先调用`Promise.resolve()`将其转为 Promise 实例。

`Promise.race()`，同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);
```

`p`的状态由第一个返回`resolve`或`reject`状态的实例决定。

`Promise.any()`，与`.race()`相似，只要有一个实例成功即返回成功，但是需要所有实例失败才会返回失败。

`Promise.resolve()`，将普通对象转为 Promise 对象。

```js
const jsPromise = Promise.resolve($.ajax('/whatever.json'));
```



### 总结

Promise是一种用于处理异步编程的方案，它是一个对象。

有三种状态：进行中、已成功、已失败。

一些常用 API：then、catch、finally、all、race、any、resolve等。



### async 函数

是一种更加简洁的 Promise 语法糖。

```js
async function fn(x){
    let n = await 123;
    return n;
}
```

**注意：**，`await`必须在`async`里使用，用在别的地方会报错。

async 函数的返回结果是一个 Promise 实例，所以它也可以使用 Promise 的一系列方法。

```js
fn(x).then(function(x){
    console.log(x); // 123
})
```

