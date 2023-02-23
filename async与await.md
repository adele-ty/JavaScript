# #.async/await
* async 和 await让我们可以用一种更简洁的方式写出基于Promise的异步行为，使用 async/await 关键字就可以在异步代码中使用普通的 try/catch 代码块，而无需刻意地链式调用 promise。  

* 使用 await 关键字可以暂停异步函数代码的执行，等待Promise解决，promise 的解决值会被当作该 await 表达式的返回值，再异步恢复异步函数的执行，任一 await 调用失败，它将自动捕获异常，async 函数执行中断，并通过隐式返回 Promise 将错误传递给调用者。  

* async函数一定会返回一个 promise 对象，async/await 可以和 Promise.all 一起使用，当我们需要同时等待多个 promise 时，我们可以用 Promise.all 把它们包装起来，然后使用 await，如果出现 error，也会正常传递，从失败了的 promise 传到 Promise.all，然后变成我们能通过使用 try..catch 在调用周围捕获到的异常。

来自 <https://zh.javascript.info/async-await>  

来自 <https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function#%E5%B0%9D%E8%AF%95%E4%B8%80%E4%B8%8B> 
