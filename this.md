this的上下文基于函数调用的情况。和函数在哪定义无关，和函数怎么调用有关。
### 全局上下文
在全局上下文（任何函数以外），this指向全局对象。
### 函数上下文
在函数内部时，this由函数怎么调用来确定。  
1. 简单调用  
由于this没有通过call来指定，且this必须指向对象，那么默认就指向全局对象。
```c
function f1(){
  return this;
}
f1() === window; // global object
```
在严格模式下，this保持进入execution context时被设置的值。如果没有设置，那么默认是undefined。它可以被设置为任意值 **（包括null/undefined/1等等基础值，不会被转换成对象）**。
```c
function f2(){
  "use strict"; // see strict mode
  return this;
}
f2() === undefined;
```
2. 箭头函数  
在箭头函数中, **this由词法/静态作用域设置**，也就是说箭头函数的 this 值是在定义时确定的，而不是在运行时确定的。它被设置为包含它的execution context的this，并且不再被调用方式影响（call/apply/bind）。
```c
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true

// Call as a method of a object
var obj = {foo: foo};
console.log(obj.foo() === globalObject); // true

// Attempt to set this using call
console.log(foo.call(obj) === globalObject); // true

// Attempt to set this using bind
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```
3. 对象的方法  
当函数作为对象方法调用时，this指向该对象。原型链上的方法跟对象方法一样，作为对象方法调用时this指向该对象。
```c
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

console.log(o.f()); // 37
```
4. 构造函数。this指向构造的实例  
5. DOM事件监听器。this自动设置为触发事件的dom元素。
### 改变this指向
1. call和apply  
Function.prototype上的call和apply可以指定函数运行时的this，并且立即执行函数。
```c
function add(c, d){
  return this.a + this.b + c + d;
}

var o = {a:1, b:3};
add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34
```
    * 当用call和apply而传进去作为this的不是对象时，将会调用内置的ToObject操作转换成对象。所以4将会装换成new Number(4)，而null/undefined由于无法转换成对象，全局对象将作为this。  
    * call和apply传递的参数，call传递参数a1、a2、a3...形式，apply必须以数组形式  
2. f.bind(someObject)会创建新的函数（函数体和作用域与原函数一致），但不会立即执行函数，而是返回一个永久改变this指向的函数。this被永久绑定到someObject，不论你怎么调用。
