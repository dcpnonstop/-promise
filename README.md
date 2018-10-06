
# 初识Promise
---
![promise](https://raw.githubusercontent.com/dcpnonstop/blogpic/master/promise.jpg)
# 1.Promise的含义
![promise](https://raw.githubusercontent.com/dcpnonstop/blogpic/master/promises.png)
> Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。
所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
###  Promise对象的两个特点：
 1.  对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
	

 - 一个 Promise有以下几种状态:  

	- [ ]  pending: 初始状态，既不是成功，也不是失败状态。
	- [ ]   fulfilled: 意味着操作成功完成。
 	- [ ]   rejected: 意味着操作失败。
	<br>
![promise](https://raw.githubusercontent.com/dcpnonstop/blogpic/master/promise-states.png)

---

# 2. Promise的基本用法

ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。

下面代码创造了一个Promise实例。

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

```
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数
# 3.Promise方法
 #### 3.1 Promise.all(iterable)
这个方法返回一个新的promise对象，该promise对象在iterable参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。（可以参考jQuery.when方法---译者注）
 #### 3.2  Promise.race(iterable)
当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象。
 #### 3.3 Promise.reject(reason)
返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法
 #### 3.4 Promise.resolve(value)
返回一个状态由给定value决定的Promise对象。如果该值是一个Promise对象，则直接返回该对象；如果该值是thenable(即，带有then方法的对象)，返回的Promise对象的最终状态由then方法执行决定；否则的话(该value为空，基本类型或者不带then方法的对象),返回的Promise对象状态为fulfilled，并且将该value传递给对应的then方法。通常而言，如果你不知道一个值是否是Promise对象，使用Promise.resolve(value) 来返回一个Promise对象,这样就能将该value以Promise对象形式使用。

# 3.Promise原型

> **属性**
Promise.prototype.constructor
返回被创建的实例函数.  默认为 Promise 函数.
---
 #### 4.1 Promise.prototype.catch(onRejected)
添加一个拒绝(rejection) 回调到当前 promise, 返回一个新的promise。当这个回调函数被调用，新 promise 将以它的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的完成结果作为新promise的完成结果.
 #### 4.2 Promise.prototype.then(onFulfilled, onRejected)
添加解决(fulfillment)和拒绝(rejection)回调到当前 promise, 返回一个新的 promise, 将以回调的返回值来resolve.
 #### 4.3 Promise.prototype.finally(onFinally)
添加一个事件处理回调于当前promise对象，并且在原promise对象解析完毕后，返回一个新的promise对象。回调会在当前promise运行完毕后被调用，无论当前promise的状态是完成(fulfilled)还是失败(rejected)

---
# 一个通俗易懂的Promise实例
```
// 定外卖就是一个Promise,Promist的意思就是承诺
// 我们定完外卖，饭不会立即到我们手中
// 这时候我们和商家就要达成一个承诺
// 在未来，不管饭是做好了还是烧糊了，都会给我们一个答复
function 定外卖(){
    // Promise 接受两个参数
    // resolve: 异步事件成功时调用（菜烧好了）
    // reject: 异步事件失败时调用（菜烧糊了）
    return new Promise((resolve, reject) => {
        let result = 做饭()
	// 下面商家给出承诺，不管烧没烧好，都会告诉你
	if (result == '菜烧好了') 
	    // 商家给出了反馈
	    resolve('我们的外卖正在给您派送了')
	else 
	    reject('不好意思，我们菜烧糊了，您再等一会')
	})
}

// 商家厨房做饭，模拟概率事件
function 做饭() {
    return Math.random() > 0.5 ? '菜烧好了' : '菜烧糊了'
}

// 你在家上饿了么定外卖
// 有一半的概率会把你的饭烧糊了
// 好在有承诺，他还是会告诉你
定外卖()
    // 菜烧好执行，返回'我们的外卖正在给您派送了'
    .then(res => console.log(res))
    // 菜烧糊了执行，返回'不好意思，我们菜烧糊了，您再等一会'
    .catch(res => console.log(res))
```

---
**参考：**

 - [promise-book](https://github.com/dcpnonstop/blogpic/blob/master/javascript-promise-book.pdf)
- [promise实例](https://zhuanlan.zhihu.com/p/29632791)
- [一步一步实现promise](https://github.com/xieranmaya/blog/issues/3)
- [阮一峰Promise对象](http://es6.ruanyifeng.com/#docs/promise)
- [MDN promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
