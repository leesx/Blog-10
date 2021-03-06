## 理解作用域

### 编译原理

编译器对代码进行编译的过程主要分为三步骤： 

* 分词/词法分析： 将由字符组成的字符串分解成（对编程语言来说）有意义的代码块，分解成的代码块称为词法单元流。

* 解析/语法分析： 将词法单元流转换成 AST 抽象语法树。

* 代码生成： 将 AST 抽象语法树转换成可执行的代码。

JavaScript 程序分为编译阶段和执行阶段，代码需要先编译后执行，编译总是发生在执行之前的几微妙内，所以对 JavaScript 引擎的性能要求很高。


### JavaScript 赋值操作

JavaScript 引擎是如何处理 JavaScript 代码的呢？比如： var a = 2;

* 首先在编译阶段，编译器会先询问作用域，在作用域集合中是否存在变量 a，如果存在则会忽略变量的声明，否则会要求作用域在当前的作用域集合中声明一个新变量，并命名为 a。

* 编译完成之后，进入到执行阶段，引擎在执行过程中，会先询问当前作用域中是否存在一个变量 a，存在的话就会使用这个变量，如果当前作用域中无法找到变量 a，就会往上一级作用域中继续查找，直到找到变量 a 或者查询完所有的作用域集合。

* 如果引擎中查找到变量 a，就会将 2 作为值赋值给变量 a。

### 引擎查找

在执行阶段，引擎是如何在作用域的配合下查找变量的？引擎的查找方式有两种： RHS 和 LHS。

* RHS: 可以理解为目的是为了获取变量的值的查询方式，比如：```console.log(a)``` ，需要获取到变量 a 的值，使用的就是对变量 a 的 RHS 查询方式。
* LHS: 可以理解为目的是为了给变量赋值的查询方式，比如： ```a = 2;```  这样的赋值操作。

### 作用域是什么

作用域其实是定义和查找变量的一套规则，用于存储变量，并且在之后可以方便的查找到变量。作用域存在层级关系，或者说是存在作用域嵌套。

引擎在执行代码时，RHS 和 LHS 查询都会从当前作用域开始，如果当前作用域中查找不到，就会逐级往上一层的作用域中查找，直到查找到了需要的变量或者到达了最顶级的作用域，引擎会终止查询。

RHS 和 LHS 查询结果差异：

* RHS 查询在作用域集合中查到不到变量，不成功的 RHS 查询会抛出 ReferenceError 异常。
* 非严格模式下，LHS 查询在作用域集合中查到不到变量，会在顶级作用域（全局作用域）内创建变量；严格模式下，会禁止自动或者隐式的创建全局变量，不成功的 LHS 查询会抛出 ReferenceError 异常。

## 词法作用域



