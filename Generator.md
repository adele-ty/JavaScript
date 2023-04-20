迭代器是一种特殊对象，每一个迭代器对象都有一个next()，该方法返回一个对象，包括value和done属性。  
生成器是函数：用来返回迭代器。  
### generators 的作用
* 迭代器(数据生产者)
每一个yield可以通过next()返回一个值，这意味着generators可以通过循环或递归生产一系列的值，因为generator对象实现了Iterable接口，generator生产的一系列值可以被ES6中任意支持可迭代对象的结构处理，两个例子，for of循环和扩展操作（...）

* 观察者(数据消费者)
yield可以通过next()接受一个值，这意味着generator变成了一个暂停执行的数据消费者直到通过next()给generator传递了一个新值

* 简化异步操作

参阅[迭代器与生成器](https://github.com/hyy1115/ES6-learning/blob/master/doc/8%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E8%BF%AD%E4%BB%A3%E5%99%A8%EF%BC%88Iterator%EF%BC%89%E5%92%8C%E7%94%9F%E6%88%90%E5%99%A8%EF%BC%88Generator%EF%BC%89.md)  
阅读[es6 Generators详解](https://segmentfault.com/a/1190000012358863)
