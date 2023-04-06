使用new关键字创建对象的过程如下：  
1. 创建一个新的空对象，该对象的原型连接到构造函数的原型对象。  
2. 将构造函数的this关键字设置为新对象。  
3. 执行构造函数，将参数传递给它，这样就可以在新对象上设置属性和方法。  
4. 返回新对象。  
```c
// 定义一个构造函数
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// 使用new关键字创建一个Person对象
var person = new Person("John Doe", 30);

// 访问对象的属性
console.log(person.name); // 输出 "John Doe"
console.log(person.age); // 输出 30
```
在上述示例中  
1. 创建了一个名为person的新对象，并将其连接到Person构造函数的原型对象。  
2. 将构造函数的this设置为新对象。  
3. 执行构造函数。构造函数使用传递给它的参数（即“John Doe”和30）来初始化新对象的name和age属性。  
4. 构造函数返回新对象，并将其分配给变量person。