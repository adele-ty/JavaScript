# Promise对象
## 含义：  
Promise代表的就是一个异步操作的最终结果，会让回调嵌套变得易于维护，职责更趋于单一，尤其是在多种异步的场景下保证执行顺序，避免回调的过度嵌套。  
## 特点：  
* 对象状态不受外界影响。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。  
* 一旦状态改变，就不会再变，任何时候都可以得到这个结果。  
## 缺点：  
* 无法取消Promise，一旦新建它就会立即执行，无法中途取消。  
* 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部，执行也不会中断。  
* 当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。  
## 基本用法：  
1. 创建Promise实例：Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。  
2. Promise.prototype.then()：then方法可以接受两个回调函数作为参数，返回一个新的Promise实例。第一个回调函数（resolve函数）是Promise对象的状态变为resolved时调用，第二个回调函数（reject函数）是Promise对象的状态变为rejected时调用。reject函数的参数通常是Error对象的实例，表示抛出的错误；resolve函数的参数除了正常的值以外，还可能是另一个 Promise 实例。  
3. Promise.prototype.catch()：如果异步操作抛出错误，就会调用catch()方法指定的回调函数。Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获。  
4. Promise.prototype.finally()：不管 Promise 对象最后状态如何，都会执行的操作。finally方法的回调函数不接受任何参数，该方法中的操作与前面Promise状态无关的，不依赖于 Promise 的执行结果。  
5. Promise.all()：Promise.all()方法接受一个数组[p1, p2, p3]作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。  
* 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。  
* 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。  
* 注意，如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法。  
6. Promise.race()：Promise.race()方法接受一个数组[p1, p2, p3]作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。  
7. Promise.allSettled()：有时候，我们希望等到一组异步操作都结束了，不管每一个操作是成功还是失败，再进行下一步操作。接受一个数组作为参数，数组的每个成员都是一个 Promise 对象，并返回一个新的 Promise 对象。只有等到参数数组的所有 Promise 对象都发生状态变更（不管是fulfilled还是rejected），返回的 Promise 对象才会发生状态变更。  
8. Promise.any()：只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态。  
9. Promise.resolve()：将现有对象转为 Promise 对象。  
* 参数是一个 Promise 实例：如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。  
* 参数是一个thenable对象：thenable对象指的是具有then方法的对象，Promise.resolve()方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then()方法。  
* 参数不是具有then()方法的对象，或根本就不是对象：如果参数是一个原始值，或者是一个不具有then()方法的对象，则Promise.resolve()方法返回一个新的 Promise 对象，状态为resolved。  
* 不带有任何参数：Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。   
10. Promise.reject()：返回一个新的 Promise 实例，该实例的状态为rejected。  
11. Promise.try()：不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管f是否包含异步操作，都用then方法指定下一步流程，用catch方法处理f抛出的错误。Promise.try就是模拟try代码块。  

来自 <https://es6.ruanyifeng.com/#docs/promise> 
