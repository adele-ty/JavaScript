# async/await
- 背景：使用promise能很好的解决回调地狱的问题，但是这种充满了.then()方法，如果处理流程复杂，那么整段代码有很多then，会导致语义化不明确。async 和 await让我们可以用一种更简洁的方式写出基于Promise的异步行为，使用 async/await 关键字就可以在异步代码中使用普通的 try/catch 代码块，而无需刻意地链式调用 promise。  

- 使用 await 关键字可以暂停异步函数代码的执行，等待Promise解决，promise 的解决值会被当作该 await 表达式的返回值，再异步恢复异步函数的执行，任一 await 调用失败，它将自动捕获异常，async 函数执行中断，并通过隐式返回 Promise 将错误传递给调用者。  

- async函数一定会返回一个 promise 对象，async/await 可以和 Promise.all 一起使用，当我们需要同时等待多个 promise 时，我们可以用 Promise.all 把它们包装起来，然后使用 await，如果出现 error，也会正常传递，从失败了的 promise 传到 Promise.all，然后变成我们能通过使用 try..catch 在调用周围捕获到的异常。

- Async/await使用了两种技术：Promise和Generator（底层实现机制——协程）

生成器运行过程：生成器Generator是可以暂停执行和恢复执行的。生成器会返回一个迭代器，
在生成器函数内部执行一段代码，遇到yield关键字，js引擎会返回关键字后面的内容给外部，并暂停该函数的执行。外部函数可以通过迭代器的next方法恢复函数的执行。

函数为什么能暂停和恢复，首先要了解协程的概念。简单来说，可把协程看成是跑在线程上的任务，线程与协程之间是'1 v N'的关系，但是在线程上同一时间只能执行一个协程任务，比如当前执行的是A协程，要启动B的话A就需要将控制权交给B，这就体现在A暂停执行，B恢复执行，我们把A叫做B的父协程。

协程的高效性：协程是完全由程序所控制，也就是在用户态执行，这样的好处就是性能得到了很大的提升，不会像切换线程那样消耗资源。

参考[异步神器async-await](https://segmentfault.com/a/1190000011526612)  
参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function#%E5%B0%9D%E8%AF%95%E4%B8%80%E4%B8%8B)
