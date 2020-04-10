# this的指向



**在JavaScript中 this 总是指向 调用它所在方法的对象。因为 this 是在函数运行时，自动生成的一个内部对象，只能在函数的内部使用。**



下面我们分几种情况深入分析 this 的用法：

### 一、全局的函数调用

```javascript
function globalTest(){
	this.name = "global this";
	console.log(this.name);
}
globalTest();  // global this
```

以上代码中，globalTest()是全局性的方法，属于全局性调用，因此 this 就代码对象 window。为了充分证明 this 是 window，对代码进行了如下更改：

```javascript
var name = "global this";
function globalTest(){
	console.log(this.name)
}
globalTest();  // global this
```

`name` 作为一个全局变量，运行结果仍然是 “global this”，说明 this 指向的是 window。在方法中我们尝试更改全局name，再次调用方法输出 “rename global this”，说明全局的name在方法内部被更改。代码如下：

```javascript
var name = "global this";
function globalTest() {
    this.name = "rename global this"
    console.log(this.name);
}
globalTest(); //rename global this
```

根据以上三段代码，我们得出结论：**对于全局的方法调用，this指向的是全局对象 window，即调用方法所在的对象**。



### 二、对象方法的调用

**如果函数作为对象的方法调用，this指向的是这个上级对象，即调用方法的对象**。在以下代码中，this指向 obj 对象。

```javascript
function showName(){
	console.log(this.name)
}
var obj = {};
obj.name = "ooo";
obj.show = showName;
obj.show();  // ooo

```



### 三、构造函数的调用

构造函数中的this指向新创建的对象本身。

```javascript
function showName(){
	this.name = "showName function";
}
var obj = new showName();
console.log(obj.name);  // showName function
```

上述代码中，我们通过 new 关键字创建一个对象的实例，new 关键字可以改变 this 的指向，将这个 this 指向对象 obj。

我们再增加一个全局的name，用以证明this指向的不是global：

```javascript
var name = "global name";
function showName(){
	this.name = "showName function";
}
var obj = new showName();

console.log(obj.name);  // showName function
console.log(name);  // global name
```

在构造函数的内部，我们对 this.name进行赋值，但并没有改变全局变量 name。



### 四、apply / call调用时的this

apply 和 call 都是为了改变函数体内部的this 指向。其具体定义如下：

**call方法：**

语法： `call(thisObj,Object)`

定义：调用一个对象的一个方法，以另一个对象替换当前对象。

说明：

call方法可以用来代替另一个对象调用一个方法。call方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。

如果没有提供 thisObj 参数，那么 Global 对象被作为 thisObj。



**apply方法：**

语法： apply(thisObj,[argArray])

定义：应用某一对象的一个方法，用另一个对象替换当前对象。

说明：

如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj，并且无法被传递任何参数。

```javascript
var value = "global value";

function FunA(){
    this.value = "AAA"
}
function FunB(){
    console.log(this.value)
}
FunB();  // global value   因为是在全局中调用FunB(),this.value指向 全局 value。
FunB.call(window); // global value，this指向 window对象，因此this.value指向全局的 value。
FunB.call(new FunA());  // AAA， this指向参数 new FunA(),即新创建的对象

FunB.apply(window)  // global value
FunB.call(new FunA())  // AAA
```

在上述代码中，this的指向在call和apply中是一致的，只不过是调用参数的形式不一样。call是一个调用参数，而apply是调用一个数组。















































































