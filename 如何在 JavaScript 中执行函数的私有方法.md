在 JavaScript 中，函数的私有方法可以通过使用闭包来实现。闭包是指在一个函数内部定义另一个函数，并返回该函数的过程。在返回的函数内部，可以访问外部函数作用域中的变量和函数，形成了一个私有作用域。通过这种方式，可以实现一个函数的私有方法。

下面是一个示例：
```
    function add(x, y) {
    // 私有方法
        function checkNumber(num) {
            return typeof num === 'number';
        }
    
        if (checkNumber(x) && checkNumber(y)) {
            return x + y;
        } else {
            return 'Error: Invalid arguments.';
        }
    }

    console.log(add(1, 2)); // 3
    console.log(add('1', '2')); // Error: Invalid arguments.    
```
在上面的示例中，checkNumber 函数是 add 函数的私有方法。它只能在 add 函数内部使用，无法从外部访问。在 add 函数内部，通过调用 checkNumber 函数来检查参数的类型。如果参数的类型是数字，则执行加法运算，否则返回错误信息。