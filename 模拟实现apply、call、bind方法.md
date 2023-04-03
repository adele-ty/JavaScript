# call
call() 方法在使用一个指定的 this 值和若干个指定的参数值的前提下调用某个函数或方法。
```c
Function.prototype.myCall = function (context, ...args) {
  context = context || window;
  context.fn = this;
  context.fn(...args);
  delete context.fn;
};
```
# apply
apply() 方法与 call() 方法类似，区别在于它接受一个数组作为参数，数组中的元素作为要调用的函数的参数。
```c
Function.prototype.myApply = function (context, args) {
  context = context || window;
  context.fn = this;
  context.fn(...args);
  delete context.fn;
};
```
# bind
bind方法的功能：  
* bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。  
* 一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。
```c
Function.prototype.bind2 = function (context) {
    if (typeof this !== "function") {
        throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);
    var fNOP = function () { };

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        // 判断对象是否由new关键字创建，是则将this指向fBound
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```
调用该方法：
```c
function oFun(name, old) {
    this.name = name
    this.old = old
    console.log(this)    //fBound {name: 'mary', old: 30}
}
var o = { value: 12 }
var fun = oFun.bind2(o, 'mary')
var cc = new fun(30)
```
参考[模拟call和apply方法](https://github.com/mqyqingfeng/Blog/issues/11)  
参考[模拟bind方法](https://github.com/mqyqingfeng/Blog/issues/12)