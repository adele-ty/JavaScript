1. 使用 Object.defineProperty 只能重定义属性的读取（get）和设置（set）行为，Proxy可以重定义更多的行为，比如 in、delete、函数调用等更多行为，除了 get 和 set 之外，proxy 可以拦截多达 13 种操作。  
2. 当使用 Object.defineProperty，我们修改原来的 obj 对象就可以触发拦截，而使用 proxy，就必须修改代理对象，即 Proxy 的实例才可以触发拦截。  
3. Object.defineProperty 是通过修改目标对象的属性描述符来实现的，只能拦截单个属性。而 Proxy 是通过代理对象来实现的，可以拦截整个对象。  
4. Object.defineProperty无法拦截数组的变化，而proxy可以拦截数组的变化。  
5. Object.defineProperty直接修改原始数据，proxy返回一个修改后的proxy实例。  

思考：为什么Object.defineProperty无法拦截数组的变化？  
答：Object.defineProperty() 是用于直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。然而，对于数组来说，这个方法并不能拦截数组数据的变化，因为数组在 JavaScript 中是特殊的对象，它们拥有一些特殊的行为。
当我们使用 Object.defineProperty() 为数组创建一个新属性或修改现有属性时，我们实际上是在操作数组对象的属性，而不是数组的元素。而push、pop、shift等方法操作的是数组的元素而不是数组的属性，因此无法被拦截。例如：
```c
const arr = [1, 2, 3];

Object.defineProperty(arr, '0', {
  set: function (newValue) {
    console.log('The first element has been changed to:', newValue);
  },
});

arr[0] = 42; // 这行代码不会产生任何输出，因为数组的索引访问并不会触发我们定义的 setter
```

认真阅读[defineProperty与proxy](https://github.com/mqyqingfeng/Blog/issues/107)
