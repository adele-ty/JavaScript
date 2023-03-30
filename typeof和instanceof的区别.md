# typeof
检测数据类型，返回一个小写字母的类型字符串  
1. 只需要一个操作数，操作数可以是简单数据类型也可以是复杂数据类型  
2. typeof undefined  //"undefined"  
3. typeof true  //"boolean"  
4. 整数、小数和NaN返回"number" //NaN是number的特殊值，表示不是一个数字  
5. 函数返回"function"  
6. null、数组和对象返回"object" //null返回object是个bug  
7. typeof检测未被声明的变量会返回"undefined"  
# instanceof
如果一个实例的原型链中出现过相应的构造函数，则instanceof返回 true  
1.  判断一个值是什么类型的**对象**  
2.  检测对象之间的关联性，通过__proto属性查找原型对象  
3.  instanceof左边必须是一个引用类型值（数组，对象，用new创建的值），右边必须是函数，返回一个布尔值  
4.  所有引用值都是Object实例，因此通过instanceof操作符检测任何引用值和Object构造函数都会返回true  
5.  如果用instanceof检测原始值，则始终会返回false，因为原始值不是对象  
6.  由于浏览器的不同，如果是正则表达式，instanceof会返回"function"或"object"  
![instanceof](/images/instanceof.png)