很多面向对象语言都支持两种继承：接口继承和实现继承。前者只继承方法签名，后者继承实际的方法。接口继承在 ECMAScript 中是不可能的，因为函数没有签名。实现继承是 ECMAScript 唯一支持的继承方式，而这主要是通过原型链实现的。

# 原型链
* 思路：每个构造函数都有一个原型对象，原型有一个属性指回构造函数，而实例有一个内部指针指向原型。**_如果原型是另一个类型的实例呢？_**那就意味着这个原型本身有一个内部指针指向另一个原型，相应地另一个原型也有一个指针指向另一个构造函数。这样就在实例和原型之间构造了一条原型链。这就是原型链的基本构想。  

代码如下：
```
    function SuperType() {
        this.property = true;
    }
    SuperType.prototype.getSuperValue = function() {
        return this.property;
    };
    function SubType() {
        this.subproperty = false;
    }
    // 继承 SuperType
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function () {
        return this.subproperty;
    };
    let instance = new SubType();
    console.log(instance.getSuperValue()); // true
```
* 默认原型  

所有引用类型都继承自 Object，这也是通过原型链实现的。任何函数的默认原型都是一个 Object 的实例，这意味着这个实例有一个内部指针指向 Object.prototype。这也是为什么自定义类型能够继承包括 toString()、valueOf()在内的所有默认方法的原因。  

* 原型与继承关系  

原型与实例的关系可以通过两种方式来确定。第一种方式是使用 instanceof 操作符，如果一个实例的原型链中出现过相应的构造函数，则 instanceof 返回 true。  

`console.log(instance instanceof Object); // true`  

确定这种关系的第二种方式是使用isPrototypeOf()方法。原型链中的每个原型都可以调用这个方法，只要原型链中包含这个原型，这个方法就返回 true。  

`console.log(Object.prototype.isPrototypeOf(instance)); // true`  

* 存在的问题  
原型中包含的引用值会在所有实例间共享。子类型在实例化时不能给父类型的构造函数传参。
# 盗用构造函数
* 思路：在子类构造函数中调用父类构造函数。因为毕竟函数就是在特定上下文中执行代码的简单对象，所以可以使用 apply()和 call()方法以新创建的对象为上下文执行构造函数。  

代码如下：
```
    function SuperType(name){
        this.name = name;
    }
    function SubType() {
        // 继承 SuperType 并传参
        SuperType.call(this, "Nicholas"); // 第二次调用 SuperType()
        // 实例属性
        this.age = 29;
    }
    let instance = new SubType(); // 第一次调用 SuperType()
    console.log(instance.name); // "Nicholas";
    console.log(instance.age); // 29 
```
* 优点：可以在子类构造函数中向父类构造函数传参
* 存在的问题：盗用构造函数的主要缺点，也是使用构造函数模式自定义类型的问题，必须在构造函数中定义方法，因此函数不能重用。此外，子类也不能访问父类原型上定义的方法，因此所有类型只能使用构造函数模式。
# 组合继承
* 思路：组合继承弥补了原型链和盗用构造函数的不足，是 JavaScript 中使用最多的继承模式。综合了原型链和盗用构造函数，将两者的优点集中了起来。基本的思路是使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。这样既可以把方法定义在原型上以实现重用，又可以让每个实例都有自己的属性。  

代码如下：
```
    function SuperType(name){
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    SuperType.prototype.sayName = function() {
        console.log(this.name);
    };
    function SubType(name, age){
        // 继承属性
        SuperType.call(this, name);
        this.age = age;
    }
    // 继承方法
    SubType.prototype = new SuperType();
    SubType.prototype.sayAge = function() {
        console.log(this.age);
    };
    let instance1 = new SubType("Nicholas", 29);
    instance1.colors.push("black");
    console.log(instance1.colors); // "red,blue,green,black"
    instance1.sayName(); // "Nicholas";
    instance1.sayAge(); // 29
    let instance2 = new SubType("Greg", 27);
    console.log(instance2.colors); // "red,blue,green"
    instance2.sayName(); // "Greg";
    instance2.sayAge(); // 27 
```
* 存在的问题：父类构造函数始终会被调用两次，一次在是创建子类原型时调用，另一次是在子类构造函数中调用。
# 原型式继承
先来看一个函数。这个 object()函数会创建一个临时构造函数，将传入的对象赋值给这个构造函数的原型，然后返回这个临时类型的一个实例。本质上，object()是对传入的对象执行了一次浅复制。
```
    function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    } 
```
示例如下
```
    let person = {
        name: "Nicholas",
        friends: ["Shelby", "Court", "Van"]
    };
    let anotherPerson = object(person);
    anotherPerson.name = "Greg";
    anotherPerson.friends.push("Rob");
    let yetAnotherPerson = object(person);
    yetAnotherPerson.name = "Linda";
    yetAnotherPerson.friends.push("Barbie");
    console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie"
```
原型式继承适用于这种情况：你有一个对象，想在它的基础上再创建一个新对象。 你需要把这个对象先传给 object()，然后再对返回的对象进行适当修改。在这个例子中，person对象定义了另一个对象也应该共享的信息，把它传给 object()之后会返回一个新对象。这个新对象的原型是 person，意味着它的原型上既有原始值属性又有引用值属性。  

ECMAScript 5 通过增加Object.create()方法将原型式继承的概念规范化了。这个方法接收两个参数：作为新对象原型的对象，以及给新对象定义额外属性的对象（第二个可选）。
# 寄生式继承
创建一个实现继承的函数，以某种方式增强对象，然后返回这个对象。  

代码如下
```
    function createAnother(original){
        let clone = object(original); // 通过调用函数创建一个新对象
        clone.sayHi = function() { // 以某种方式增强这个对象
            console.log("hi");
        };
        return clone; // 返回这个对象
    } 
    let person = {
        name: "Nicholas",
        friends: ["Shelby", "Court", "Van"]
    };
    let anotherPerson = createAnother(person);
    anotherPerson.sayHi(); // "hi"
```
# 寄生式组合继承
寄生式组合继承的基本思路是，使用组合继承的方式实现父类的属性和方法的继承，然后再通过寄生式继承的方式对继承的结果进行优化，去掉组合继承中的缺点。寄生式组合继承解决了组合继承中父类构造函数被调用两次的问题，可以算是引用类型继承的最佳模式。  

代码如下
```
    function inheritPrototype(subType, superType) {
        let prototype = object(superType.prototype); // 创建对象
        prototype.constructor = subType; // 增强对象
        subType.prototype = prototype; // 赋值对象
    } 
    function SuperType(name) {
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    SuperType.prototype.sayName = function() {
        console.log(this.name);
    };
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    inheritPrototype(SubType, SuperType);
    SubType.prototype.sayAge = function() {
        console.log(this.age);
    }; 
```
在inheritPrototype函数中，使用 Object.create() 创建一个父类原型对象的副本 prototype ，该对象后续将作为 subType 的原型对象，从而实现继承。接下来需要将 prototype 的构造函数重新设置为 subType 构造函数，否则将会指向 superType 构造函数，最后将 subType 的原型指向 prototype 实现继承。
