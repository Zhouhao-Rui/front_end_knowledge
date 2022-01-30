# Javascript

## 1. 浏览器工作原理

Java, C, C++ 编译成可执行文件

javascript 源代码解释，再执行

-- 如何在浏览器中执行？

```
输入域名 -》ip地址 -〉index.html -》解析script标签-〉下载js文件-》浏览器运行文件
```

-- 浏览器内核？

Gecko (firefox, Netscape)

Trident (IE4 - IE11)

Webkit (chrome, safari)

Blink (Google, Chrome, Opera)

- layout engine
- rendering engine

-- 浏览器渲染过程

```
Html - > html parser -> html dom tree -> attachment -> render Tree (layout) -> painting -> display

Style sheet -> Css parser -> style rules
```

-- js引擎

将JavaScript解析成为machine code，最后被cpu执行

SpiderMonkey

Chakra (IE)

JavascriptCore(webkit)

V8 (Chrome)

-- webkit

Webcore (HTML 解析，布局，渲染)

JavascriptCore (解析，执行JavaScript代码)

-- v8

v8可以独立运行（Chrome），也可以嵌入到任何C++库中（Node）

```
Javascript代码 -》词法分析 [tokens: {type: 'keyword', value: 'const' ...}]，语法分析  
-〉AST (抽象语法树) [VariableDeclaration, Identifier...] => bytecode（TurboFan） =》machine code
```

Bytecode?

因为不同的cpu有不同的架构，machine code会有区别，所以需要先转成字节码，再转成不同的cpu指令

TurboFan？

如果一个函数多次执行，则会优化成为machine code，否则转为assemble language

Deoptimization？

如果函数类型不同，会反向转为bytecode，重新转化

所以Typescript的效率会高很多

PreParser?

LazyParser, 必要的函数进行预解析

全量解析(full parser)需要在运行时再解析

## 2. GO，VO，AO

代码解析过程？

* --代码解析，v8引擎内部创建一个对象

  var GlobalObject(GO) = {

  console, Math, String, Date, Object, window:this....

  }

 * --执行代码

   v8引擎为了执行函数，内部会有执行上下文栈（execution Context Stack）

   ECStack -> 函数的进栈和出栈

   为了执行全局代码，创建一个全局执行上下文 (Global execution Context)

   Global execution Context

   -- VO:GO(variable object)

   ```js
   var name = "rzh"
   
   /**
   * 作用域提升
   */
   console.log(num1) // undefined
   var num1 = 1
   
   console.log(window) // window == globalObject
   ```

   -- 全局函数

   - 开辟一块空间来存储函数

   ​	[[scope]]: 父级作用域 (全局作用域)

   ​	函数执行的代码块

   在GO中存储的是函数的引用地址

   - 函数执行上下文(functional execution context)

     -- VO:AO (active object)

     ```js
     function foo() {
     	var a='rz',
     	var b='b'
     }
     ```

     a和b会被提升到AO，编译阶段会被解析成为undefined

     执行完就会被销毁，之后重新再开辟空间

   --全局变量和函数变量冲突？

   ```js
   var name = "rzh"
   
   function foo() {
   	console.log(name)
   }
   ```

   AO 中的name应该是undefined，之后向上级作用域进行查找（作用域链）

   scope （AO + GO）

### 2.1. 变量环境和记录

每一个执行上下文会被关联到一个环境变量（variable object），源代码中的变量或者函数的声明，会被作为属性添加到variable object中

对于函数来说，参数也会被添加到variable object中

**variable environment** **variable record**

### 2.2 作用域提升（scope hoisting）面试题

```javascript
/**

 *  200
 *  先执行GO compiler， n = undefined
 *  再顺序执行foo()和console.log(n)
 *  GO n -> 200
    */
    var n = 100
    function foo() {
    n = 200
    }

foo()
console.log(n)

/**

 * 先执行Compiler, 函数自己AO有n，n为undefined
 * 再执行foo（）
 * undefined 200
   */

function foo() {
    console.log(n)
    var n = 200
    console.log(n)
}

var n = 100
foo()

/**

 * 首先foo2()创建FEC和AO对象n = 200，
 * 然后foo1()创建FEC和AO对象空
 * 所以n会查找到GO对象n = 100
 * 最后n查找global object = 100
   */
   var n = 100

function foo1() {
    console.log(n) //100
}

function foo2() {
    var n = 200
    console.log(n) // 200
    foo1()
}

foo2()
console.log(n) //100

/**

 * 编译阶段是不管return的，所以AO还是会有a，当执行阶段不赋值
 * return只有在执行阶段才会生效
 * a =》 100
 * */
   var a = 100

function foo() {
    console.log(a) // 100
    return 
    var a = 100
}

foo()

/**

 * 如果m没有类型，m会加入到GO
 * 如果m是var，加入到AO
   */

function foo() {
    m = 100
}

foo()
console.log(m)

/**

 * b = 10
 * var a = 10
   */

function foo() {
    var a = b = 10
}

foo()
console.log(b) // not defined
console.log(a) // 10
```

## 3. 内存管理	

代码执行分配内存（disk -》memory -〉CPU）

手动管理内存(C, C++, OC malloc, free)

自动管理内存

-- 分配申请内存空间 var obj = {}

-- 使用申请到的内存 var obj = {name: 'rz'}

-- 不需要使用的时候，进行释放 

自动管理内存(Java, javascript, python..)

### 3.1 js内存管理

- 在声明变量时自动创建内存
- 基本数据类型，直接在栈空间进行分配
- 复杂数据类型（function，object），用heap空间进行分配，然后保存地址，引用数据
- 垃圾回收机制（garbage collection GC）垃圾回收器

GC算法

引用计数 retain counter，当引用计数为0时，就开始free memory

- 循环引用 -》内存泄漏

标记清除 root object，从root开始查找，如果unreachable，则这个对象是不可用的

## 4. 闭包（clousure）

js中函数是一等公民

函数可以作为另外一个函数的参数，也可以作为另外一个函数的返回值来使用

高阶函数（high-order function）：如果一个函数能够接受另外一个函数作为参数

-- forEach, map, find, reduce, filter

当捕捉闭包的时候，它的自由变量会被确定，这样即使脱了捕捉的上下文，它也能照样运行

一个函数和对其周围状态的引用捆绑在一起，这样的组合就是闭包

一个内层函数可以访问到外层函数的作用域

```javascript
function foo() {
    var name = "rzh"
    function bar() {
        console.log("bar " + name)
    }
    return bar 
}

var fn = foo()
fn()
```

在foo执行完之后本来foo的AO本来应该销毁，但是内部bar function还能够访问foo的AO，这样就形成了闭包

闭包：函数+可访问的自由变量（bar + name）

上层作用域是在parser阶段就被确定了

- 如果函数之间的引用一直存在，但是只调用了一次或者有限次，那么会内存泄漏
- fn = null

```javascript
function foo() {
	var name = "rzh"
	var age = 12
	
	function info() {
		console.log(name)
	}
	return info
}

var fn = foo()
fn()
```

- 理论上这里虽然没用到age，但是依旧不会被销毁，因为foo的AO对象有引用，AO对象不会删除任何属性

- 但是js engine自己做了优化，不用的属性会被销毁

## 5. This

全局的this指向的是window对象，

Node环境下是一个空对象{}

```javascript
function foo() {
    console.log(this) // window
}

// 直接调用函数
foo()

// 创建一个对象，对象中函数指向foo
var obj = {
    name: 'rzh',
    foo: foo // obj对象
}

obj.foo()

// apply 调用
foo.apply('123') // string
```

this的指向和函数的位置无关，和调用的方式有关

### 5.1 默认绑定

函数调用没有绑定到对象上面，指向window

```javascript
function foo() {
    console.log(this) // window
}

function foo1() {
    foo()
    console.log(this) //window
}

function foo2() {
    foo1()
    console.log(this) // window
}

foo2()

/**
* 在被调用的时候才会分配this，所以this在被调用的时候没有绑定对象，window
*/
var obj = {
    foo: function () {
        console.log(this)
    }
}

var bar = obj.foo
bar()
```

### 5.2 隐式绑定

通过某个对象进行调用

**对象发起的调用**

对象会给绑定到function的context中

```javascript
function foo() {
	console.log(this)
}

var obj = {
	name: 'rzh',
	foo: foo
}

obj.foo() //obj
```

### 5.3 显式绑定

call和apply直接改变this的指向

call和apply只有传参数的区别，一个是剩余参数，一个是array

bind直接绑定this

```javascript
function sum(num1, num2) {
    console.log(num1 + num2, this)
}

sum.call("call", 20, 30)
sum.apply("apply", [20, 30])

var newFunc = sum.bind('aa', 20, 30)
newFunc()
// newFunc() 的默认绑定和显示绑定冲突，显示绑定的优先级更高
```

### 5.4 new 绑定

```javascript
/**
 * 
 * @param {*} name 
 * @param {*} age 
 */
function Person(name, age) {
    this.name = name
    this.age = age
}

var p = new Person("rzh", 18)
```

这样会先创建一个对象，然后绑定this

### 5.5 一些常见的内置函数的this指向

```javascript
说明是直接调用的fn()
setTimeout(function (){
    console.log(this) //window
}, 2000)

说明是boxDiv.click()
boxDiv.onclick = function () {
    console.log(this) // <div></div>
}

收集到依赖数组中，然后使用fn.call()
boxDiv.addEventListener('click', function () {
  console.log(this) // <div></div>
})

第二个参数是可以改变this的指向的，说明内部肯定是有call的
thisArgs?
forEach, map, filter..
var vals = [1, 2, 3]
vals.forEach(function (val) {
    console.log('forEach', this) // string abc
}, "abc")
```

### 5.6 函数调用的优先级

默认绑定的优先级最低

显示绑定高于隐式绑定（call，apply 》obj.fun()）

new绑定的优先级隐式绑定

new的优先级高于显示绑定

### 5.7 特殊的绑定

- Apply(null)

apply(undefined)

这时会自动将this绑定成window对象

- 间接函数引用

```javascript
var obj1 = {
    name: 'obj1',
    foo: function() {
        console.log(this)
    }
}

var obj2 = {
    name: 'obj2'
};

(obj2.bar = obj1.foo)() // window
```

### 5.8 箭头函数

箭头函数内部是没有this和arguments的

```javascript
const age = () => 18
const info = () => ({name: 'sd', age: 12})
```

箭头函数不适用四种规则，它的this决定于上层作用域

```javascript
// 模拟网络请求，此时setTimeout里面只能用箭头函数，因为他会搜索外层作用域getData，它的this是mockNetwork对象，因为mockNetwork对象调用了getData
// 否则需要在外层作用域保存this
var mockNetwork = {
    data: [],
    getData: new Promise(function (resolve, reject) {
        setTimeout(() => {
            this.data = [1, 2]
            resolve()
        }, 2000)
    })
}

mockNetwork.getData.then(res => {
    console.log(this.data)
})
```

### 5.9 面试题

```javascript
/**
* 关于上级作用域，只看是谁调用的，不要看compile之前的代码
*/
var name = "window"
var person = {
    name: 'person',
    sayName: function() {
        console.log(this.name)
    }
}

function sayName() {
    var sss = person.sayName
    sss() // window
    person.sayName() // person
    (person.sayName)() // person ==> person.sayName()
    (b = person.sayName)() // window
}
——————————————————————————————————————————————————————————
var name = "window"

var person1 = {
    name: 'person1',
    foo1: function () {
        console.log(this.name)
    },
    foo2: () => {
        console.log(this.name)
    },
    foo3: function() {
        return function () {
            console.log(this.name)
        }
    },
    foo4: function () {
        return () => {
            console.log(this.name)
        }
    }
}

var person2 = {name: 'person2'}

person1.foo1() // person1
person1.foo1.call(person2) // person2
person1.foo2() // window 上层作用域是global
person1.foo2.call(person2) // window

person1.foo3()() // window 等于是默认绑定
person1.foo3.call(person2)() // window 等于是默认绑定
person1.foo3().call(person2) // person2

person1.foo4()() // person1 上层作用域是foo4
person1.foo4.call(person2)() // person2
person1.foo4().call(person2) // person1 因为call不绑定箭头函数
——————————————————————————————————————————————————————————————————
var name = 'window'
function Person (name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  },
  this.foo2 = () => console.log(this.name),
  this.foo3 = function () {
    return function () {
      console.log(this.name)
    }
  },
  this.foo4 = function () {
    return () => {
      console.log(this.name)
    }
  }
}
// person1 = {name: 'person1', foo1(), ....}
var person1 = new Person('person1')
// person2 = {name: 'person2', foo1(), ....}
var person2 = new Person('person2')

person1.foo1() // person1
person1.foo1.call(person2) // person2

person1.foo2() // person1 上层作用域是Person
person1.foo2.call(person2) // person1 箭头函数没有call

person1.foo3()() // 独立函数调用 window
person1.foo3.call(person2)() // 独立函数调用 window
person1.foo3().call(person2) // person2

person1.foo4()() // person1 上层作用域是Person
person1.foo4.call(person2)() // person2
person1.foo4().call(person2) // 箭头函数没有call， 所以是person1
——————————————————————————————————————————————————————
var name = 'window'
function Person (name) {
  this.name = name
  this.obj = {
    name: 'obj',
    foo1: function () {
      return function () {
        console.log(this.name)
      }
    },
    foo2: function () {
      return () => {
        console.log(this.name)
      }
    }
  }
}
// Person {name: 'person1', obj: {name: 'obj', foo1(), foo2()}}
var person1 = new Person('person1')
// Person {name: 'person2', obj: {name: 'obj', foo1(), foo2()}}
var person2 = new Person('person2')

person1.obj.foo1()() // window 独立函数调用
person1.obj.foo1.call(person2)() // window 独立函数调用
person1.obj.foo1().call(person2) // person2

person1.obj.foo2()() // 箭头函数上级作用域 obj 因为是被obj调用的
person1.obj.foo2.call(person2)() // person2 上层作用域被绑定成了person2
person1.obj.foo2().call(person2) // obj 箭头函数不绑定this，所以是被obj调用的

```

### 5.10 手动实现call，apply和bind

```javascript
// 所有函数添加zhCall方法

Function.prototype.zhCall = function (thisArg, ...args) {
    var fn = this
    // type = arguments[0].constructor
    // type.prototype.fn = fn
    // arguments[0].fn()

    thisArg = thisArg ? Object(thisArg) : window
    thisArg.fn = fn
    thisArg.fn(...args)
    delete thisArg.fn
}

Function.prototype.zhApply = function (thisArg, args) {
    var fn = this
    thisArg = thisArg ? Object(thisArg) : window
    thisArg.fn = fn
    args = args || []
    thisArg.fn(...args)
    delete thisArg.fn
}

Function.prototype.zhBind = function(thisArg, ...arrayArgs) {
    var fn = this
    thisArg = (thisArg !== undefined && thisArg !== null) ? Object(thisArg) : window
    return function(...args) {
        thisArg.fn = fn
        finalArgs = [...arrayArgs, ...args]
        var result = thisArg.fn(...finalArgs)
        delete thisArg.fn
        return result
    }
}

function foo() {
    console.log(this)
}

function sum(num1, num2) {
    console.log(this, num1 + num2)
}

foo.zhCall(123)
sum.zhCall("123", 20, 30)

sum.zhApply("123", [20, 30])

var bar = sum.zhBind("123", 20)
bar(30)
```

## 6. Arguments

arguments是一个类数组，本质上是一个对象

- 获取参数的长度（arguments.length）
- 索引值获取具体的参数
- callee 获取到当前的函数

```javascript
三种方法将arguments转为array
/**
* slice默认便利一个iterator，转为数组
*/
var newArr2 = Array.prototype.slice.call(arguments)

/**
* Es6
*/
var newArr3 = Array.from(arguments)

/**
* 展开运算符
*/
var newArr4 = [...arguments]
return newArr4
```

箭头函数是没有arguments，回到上层作用域当中找

```javascript
function foo(num1, num2) {
    return () => {
        console.log(arguments)
    }
} 

foo(1, 2)() // arguments {'0': 1, '1': 2}
```

## 7. Javascript 函数式编程

### 7.1 纯函数

- 函数在相同的输入时，需要有相同的输出
- 输入和输出与外部的变量影响无关，只和输入值是有关的
- 不能产生副作用

slice和splice

slice是新建了一个空数组，将对应的sub array放入到数组中，所以这是一个纯函数

splice是直接对原数组进行修改，这个修改就是产生了副作用

- 只需要关心函数的实现逻辑，输入值和输出值都不需要改变
- 比如react的函数式组件里面的props，不能改变props的值，只能拿到props的值

### 7.2 柯里化

只传递给函数一部分参数来调用他，让他返回一个函数来处理另外的参数

```javascript
function foo() {
	return function (m) {
		return function (n) {
			return function (x) {
				....
			}
		}
	}
}
```

- **让每个函数的职责变得单一，而不是将一大堆的任务全部交给一个函数去处理** single responsibility principle
- **逻辑复用，定制一些方法** reuse some features with functions

```javascript
function makeAdder(count) {
    return function (number) {
        return count + number
    }
}

var adder5 = makeAdder(5)
adder5(10)
adder5(100)
```

```javascript
/**
 * 柯里化函数的实现
 */
var zhCurrying = function (fn) {
    function curried(...args) {
        // 判断已经接收的参数和需要接收的参数数量是否一致
        // fn.length 表示函数的参数个数
        if (args.length >= fn.length) {
            // 保证this和fn的this保持一致
            return fn.apply(this, args)
        } else {
            // 没有达到参数个数时，返回一个函数拼接参数
            // 递归用curreid判断是否达到了函数的参数值
            return function (...args2){
                return curried.apply(this, args.concat(args2))
            }
        }
    }
    return curried
}

var curryAdd = zhCurrying(sum)
console.log(curryAdd(10, 20)(30))
```

### 7.3. 组合函数

一个数据可以将函数依次调用的时候，就可以成为一个组合函数

```javascript
/**
*手动实现compose函数
*/
function zhCompose(...fns) {
    var length = fns.length
    for (var i = 0; i < length; i++) {
        if (typeof fns[i] !== 'function') {
            throw new TypeError('please input function')
        }
    }

    function compose(...args) {
        var index = 0
        var result = length ? fns[index].apply(this, args) : args
        while (++index < length) {
            result = fns[index].call(this, result)
        }
        return result
    }
    return compose
}

var new_fn = zhCompose(double, square)
console.log(new_fn(20))
```

## 8. javascript 知识补充

### 8.1 with语句

with是有自己的作用域的，有一个js对象的AO

```javascript
obj = {message: 'hello world'}
var message = '123'
function message() {
    function bar() {
        with(obj) {
            console.log(message) // 上层作用域是with
        }
    }
    bar()
}
```

可以在上下文中明确指定一个变量

### 8.2 Eval函数

可以将string转化成JavaScript代码

- 可读性非常差
- 因为是plain text，容易被篡改，造成安全隐患
- 字符串需要解释器进行解析，但是js代码会被js引擎进行词法分析，语法分析进行相关的优化

### 8.3 严格模式

strict mode，避免sloppy code，消除slient error

让js引擎执行代码时有更好的优化

避免使用保留字（class等）

- 意外创建全局变量

  ```javascript
  message = "13"
  ```

- 不允许函数有相同的参数名

- 静默错误

  ```javascript
  true.123 = "123"
  Object.defineProperty(obj, "name", {writeable: false})
  ```

- 不允许使用八进制的数字

- with函数不可以被使用

- eval函数不能向上引用变量了

- **自执行函数(默认绑定)会指向undefined**

  ```javascript
  "use strict"
  function foo() {
  	console.log(this) // undefined
  }
  
  foo()
  ```

- setTimeout还是指向window，实现是fn.apply(window)

## 9. javascript 面向对象

### 9.1 对象的两种定义形式：

```javascript
var obj = new Object()
obj.name = "rzh"
obj.rest = function () {
    console.log('resting...')
}
obj.working = function () {
    console.log('working...')
}

// 自变量形式
var obj2 = {
    name: 'rzh',
    age: 18,
    rest: function () {
        console.log('resting...')
    },
    working: function () {
        console.log('working...')
    }
}
```

### 9.2 对象的属性

- 数据类型描述符 (data properties)
- 存取类型描述符 (accessor properties)

|                     | Configurable | Enumerable | Value | Writable | Get  | Set  |
| :------------------ | ------------ | ---------- | ----- | -------- | ---- | ---- |
| data properties     | y            | y          | y     | y        | n    | n    |
| accessor properties | y            | y          | n     | n        | y    | y    |

- **Configurable: 可以通过delete删除属性**
- **Enumerable: 是否可以通过遍历得到这个属性**
- writable：是否可以修改这个属性
- value：默认是undefined

存储描述符

隐藏私有属性，不希望暴露给外部

```javascript
var obj3 = {
    name: 'rsz',
    _job: 'student',
  	/**
  	相当于getter和setter
  	set job(val) {
      this._job = val
    },
  	get job() {
      return this._job
    }
    */
}

Object.defineProperty(obj3, 'job', {
    enumerable: true,
    configurable: true,
    get() {
        return this._job
    },
    set(value) {
        this._job = value
    }
})

console.log(obj3.job)
```

数据劫持

```javascript
Object.defineProperty(obj3, 'job', {
    enumerable: true,
    configurable: true,
    get() {
        foo()
    },
    set(value) {
        this._job = value
        bar()
    }
})
function foo() {
    console.log('GET...')
}
function bar() {
    console.log('SET...')
}
obj3.job // GET...
obj3.job = 'army' // SET...
```

- 查看属性的描述符

```javascript
Object.getOwnPropertyDescriptor(obj, "name")
Object.getOwnPropertyDescriptors(obj)
```

- 阻止对象继续扩展属性

```
Object.preventExtensions(obj)
// seal 封条
Object.seal(obj) // 所有的属性的configurable 为false
Object.freeze(obj) // 所有属性的writable 为false
```

### 9.3 对象的创建

- 工厂模式，工厂方法，通过这个工厂生产不同的对象

抽离出相同的部分，其他部分通过传参数

```javascript
function createPerson(name, age) {
    return {
        name,
        age,
        eating() {
            console.log(this.name + 'eating')
        }
    }
}

var p1 = createPerson('zhangsan', 20)
var p2 = createPerson('lisi', 30)
```

**但是现在p1,p2都是object类型，不是具体的类**

- 构造函数（在对象创建的时候调用的方法）

和普通函数一样，但是如果new关键字调用的话，就是构造函数

- 在内存中先创建一个对象
- 对象内部prototype赋值为构造函数的prototype
- 构造函数的this绑定到了这个对象上
- 执行函数体内部的代码
- 返回这个对象

```javascript
/**
 * constructor
 * this = Person{}
 */
function Person(name, age) {
    this.name = name
    this.age = age

    this.eating = function () {
        console.log(this) // Person {name: 'zhangsan', age: 20, eating: [Function eating]}
        console.log(this.name + ' eating..')
    }
}

var p = new Person('zhangsan', 20)
p.eating()
```

构造函数的缺点：内部函数在不同对象创建时会被重复创建

### 9.4 对象的原型

- 对象的隐式原型

当一个对象获取某个属性时，会触发getter的操作

1. 在当前对象中查找对应的属性，如果找到直接使用
2. 如果没有找到，通过原型链进行查找

这样可以方便抽出相同的部分，进行组合和继承

```javascript
var obj5 = {name: 'rzh'}
obj5.__proto__.age = 18
console.log(obj5.age)
```

- 函数的显式原型

```javascript
fn.prototype
```

在new 操作的时候，创建出来的```__proto__``` 会引用构造函数的prototype属性

所以函数的prototype和``` __proto```是相同的

不管修改对象的原型还是函数的原型，其实修改的都是构造函数的原型对象

- contructor属性

原型上的contructor 属性指向了函数本身

fn.prototype.contructor.prototype.contructor...

- 利用原型抽离一些方法

```javascript
function Animal(name, age) {
    this.name = name
    this.age = age
}

Animal.prototype.eating = function () {
    console.log(this.name + 'eating')
}

var dog = new Animal('dog', 2)
var cat = new Animal('cat', 3)
dog.eating()
cat.eating()
```

### 9.5 面向对象的三大特性

- 封装
  - 将所有的属性和方法写到一个类中，被称为封装
- 继承
  - 将重复的代码逻辑放到父类当中，让子类进行代码复用
  - 继承是多态的前提
- 多态

原型链：原型的上面还可以有原型，直到找到Object的原型

```javascript
var obj = {}
obj.__proto__ = {}
obj.__proto__.__proto__ = {}
```

Object的默认的原型是[Object null prototype]，继续查找就是null

```javascript
/**
 * Object.prototype 所有的属性都被设置了enumerable false，所以不可见
 * 
 * constructor 指向Object
 */
console.log(Object.prototype.constructor)

Object.defineProperty(Object.prototype, 'constructor', {
    enumerable: true,
    configurable: true
})

console.log(Object.prototype)

console.log(Object.getOwnPropertyDescriptors(Object.prototype))
```

所有的构造函数都继承了Object

```javascript
function Person(name) {
	this.name = name
}

console.log(Person.prototype.__proto__) // Object
```

#### 9.5.1 继承

1. 原型链继承

```javascript
// 父类
function Person() {
    this.name = "lisi"
}

Person.prototype.eat = function () {
    console.log(this.name + " eating")
}

// 子类
function Student() {
    this.name = "zhangsan"
    this.sno = "123"
}

Student.prototype = new Person()

Student.prototype.study = function () {
    console.log(this.name + " studying")
}
console.log(Student.prototype)
var stu = new Student()
/**
 * student调用了eat，所以this是指向stu的
 */
stu.eat()
```

缺点：

- 继承的属性不可见

- 多个子类继承同一个父类之后，如果同时修改prototype，会出现问题

  ```javascript
  stu1.name = "123" // 不会影响stu2
  stu1.friends.push("123") // 会影响stu2
  ```

- 很难传递参数，比如student想new出来一个有name的对象，但是Student的构造函数里面没有name

2. 改进的原型链继承，使用了constructor stealing，将子类的this绑定到父类中

```JavaScript
function Person(name, age, friends) {
    this.name = name
    this.age = age
    this.friends = friends
}

Person.prototype.eat = function () {
    console.log(this.name + " eating")
}

// 子类
function Student(name, age, friends, sno) {
    // 改变this的指向，Person中的this变成了Student类
    Person.call(this, name, age, friends)
    this.sno = sno
}
Student.prototype.study = function () {
    console.log(this.name + " studying")
}
var stu = new Student('hah', 18, ['fd'], 11)
```

缺点：

- 原型对象会多出属性，不应该在原型上出现
- 原型会被多次调用

3. 原型式继承函数（prototypal inheritance in javascript）

```javascript
/**
* 创建一个对象，并且原型是另外一个对象
*/
var obj = {
    name: 'zhouhao',
    age: 18
}

var info = {

}

function createObject(obj) {
    var newObj = {}
    Object.setPrototypeOf(newObj, obj)
    return newObj
}

/**
 * 使用函数的prototype，new这个函数得到对象
 */
function createObject__(obj) {
    function Fn() {}
    Fn.prototype = obj
    return new Fn()
}

Object.create(obj)

var info = createObject(obj)
```

4. 寄生式继承

使用工厂函数，继承原型

```javascript
/**
 * 寄生式继承
 */
var obj = {
    running: function () {
        console.log("running")
    }
}

function factoryObj(person, name) {
    var stu = Object.create(person)
    stu.name = name

    return stu
}

var stu = factoryObj(obj, 'haha')
console.log(stu.name)
```

5. 寄生组合式继承

使用了一个中间人对象，避免重复使用Person

```javascript
function oPerson(name, age, friends) {
    this.name = name
    this.age = age
    this.friends = friends
}

oPerson.prototype.running = function () {
    console.log('running')
}

function oStudent(name, age, friends, sno, score) {
    oPerson.call(this, name, age, friends)
    this.sno = sno
    this.score = score
}

/**
 * 相比直接使用Person，这里利用一个中间人继承了Person的prototype，
 * student继承了这个中间人
 * 避免了重复使用Person
 * 因为要保证constructor一致，使用defineProperty定义一个属性
 */
var intermidater = Object.create(oPerson.prototype)
oStudent.prototype = intermidater
Object.defineProperty(oStudent, 'constructor', {
    enumerable: false,
    configurable: true,
    writable: true,
    value: oStudent
})

oStudent.prototype.study = function () {
    console.log('study')
}
var stu = new oStudent('zhouhao', 20, ['gg'], 208, 12)
stu.running()
console.log(stu.name)
stu.study()
```

```javascript
function inherit(subType, superType) {
    var intermidater = Object.create(superType.prototype)
    subType.prototype = intermidater
    Object.defineProperty(subType.prototype, 'constructor', {
        enumerable: false,
        configurable: true,
        writable: true,
        value: subType
    })
}
```

#### 9.5.2 Object方法补充

判断是否是自己的属性

```javascript
console.log(info.hasOwnProperty('address'))
console.log('address' in info)
```

instanceof 查看当前对象是否出现在原型链上

```javascript
var stu = new Student()
// stu.__proto__ = Student.prototype
console.log(stu instanceof Student) // true
console.log(stu instanceof Person) // true
console.log(stu instanceof Object) // true
```

#### 9.5.3 对象、函数之间的关系

函数本质上也是一个对象，函数有显示原型对象 Foo.prototype，同时也有隐式原型Foo.__ proto__

Foo.prototype = { constructor }

Foo.__ proto__ 是new Function() 出来的，本质上是Function.prototype

#### 9.5.4 多态 (Polymorphism)

不同数据类型提供单一的接口，或者使用单一符号表示不同的数据类型

- 必须有继承
- 必须重写方法
- 父类引用指向子类对象

但是因为javascript函数是第一公民，所以会有不同形式的多态

### 9.6 ES6 class对象

ES6的class对象本质上是ES5 function的变形，prototype也是一个对象，含有constructor和__ photo__

```javascript
// 类的声明
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
}

// 类的表达式
var Animal = class {

}

var p = new Person("rzh", 18)
console.log(p)
```

constructor函数：

1. 在内存中创建一个空的对象
2. 这个对象的prototype属性会被赋值为这个类的prototype
3. 构造函数内部的this，会指向创建出来的新对象
4. 执行构造函数内部的代码
5. 构造函数没有返回非空对象，返回创建出来的对象

属性劫持

```javascript
class Person {
	constructor() {
		this._address = "default"
	}
  
  get address() {
    console.log("data thief")
    return this._address
  }
  
  set address(new_address) {
    console.log("data thief", new_val)
    this._address = new_address
  }
}
```

类方法

```javascript
/**
     * 静态方法
     */
static createPerson() {
  return new Person()
}
```

Super()

- 构造函数中调用父类的构造方法，否则会报错

- 在函数重写时，调用原来的方法

- 继承静态方法

Mixins

使用函数和继承进行混入，实现多继承

```javascript
function MixinRunner(BaseClass) {
    return class extends BaseClass {
        running() {
            console.log('running')
        }
    }
}
```

可以类比React的High Order component

```javascript
// 柯里化
function connect(state, dispatch) {
	return function EnhanceHoc(BaseComponent) {
    // 混入，最终返回了一个新的class component
		class EnhanceComponent extends Component {
      render() {
        return (
          <BaseComponent ...props ...dispatch>
        )
      }
		}
	}
}
```

## 10. ES6语法

### 10.1 property shorthand

```javascript
var obj = {
	name: name, => name
}
```

### 10.2 method shorthand

```javascript
var obj = {
	bar: function() {
	
	} => bar() {}
}
```

### 10.3 computed property name

```javascript
var obj = {
	[firstName + lastName]: 'haha'
}

obj[firstName + lastName] = 'haha'
```

### 10.4 Destructuring

```javascript
var obj = {
	name: '123',
	age: 123
}
var { name } = obj // '123' 

var arr = ['123', '12', '1']
var [item1, item2, item3] = arr
var [item1, ...item2] = arr
```

### 10.5 let 和 const

const本质上是传递的值不可以修改，但是如果传递的是引用对象的话，是可以修改的

```javascript
const obj = {
	foo: 'foo'
}
可以修改foo，但是不能直接重写obj, i.e., const obj = {}
因为引用对象只是保存了一个内存地址
```

- 通过let和const定义的对象，不能重复声明
- let和const没有作用域提升的(scope hoisting)，虽然也能够提前解析，但是不会被访问

```javascript
console.log(hoist) //undefined
console.log(_hoist) //Error
/**
* hoist在parse阶段被放入了内存空间 hoist = undefined
*/
var hoist = '123'
/**
* _hoist在parse阶段也会放入调用栈，但是不能被访问，直到access的时候才会被调用。
*/
const _hoist = '1'
```

**ES6下的GO，VO等**

每个execution context关联到variable environment上，在执行代码中变量和函数的声明作为environment record添加到环境变量中

VE -> 一个VaribaleMap(HashMap) <- GO 

variables_: VariableMap

Window和GO不再指向同一个_variable，因为ES6有了新特性，Window会指向另外一个object

**如果用var定义，会直接添加到window上，很容易出现bug**

### 10.6 块级作用域

ES5 中是没有块级作用域的，只有三个东西会形成作用域，function，with和全局作用域

```javascript
/**
 * 三个作用域，全局+两个function作用域
 */
function foo() {
    function bar() {

    }
}
```

ES6中开始有块级作用域，对const，let，function和class有效

```javascript
{
	let name = "123"
	function demo() {
		
	}
	class Person {}
}
```

浏览器为了兼容ES5，所以function不会报错

if，switch和for都是具备了块级作用域，所以在if，switch和for中如果定义了let或者const对象，都不能访问

块级作用域的好处：

```javascript
/** 
* 永远都是4，因为function会到全局作用域中去找i，此时i已经是4了
*/
html..
<button>1</button>
<button>2</button>
<button>3</button>
<button>4</button>

js..
var btns = document.querySelectorAll('button')

for (i = 0; i < btns.length; i++) {
	btns[i].onclick = function () {
		console.log(i) // 4
	}
}

/**
* 解决方法1: 使用立即执行函数，添加一个作用域
* 此时n向外层的function查找，查找到n值，n值是每次i++后都会重新传递给function的参数
* 闭包
*/
for (var i = 0; i < btns.length; i ++) {
    (function (n) {
        btns[n].onclick = function () {
            console.log(n)
        }
    })(i)
}

/**
* 解决方法2: 使用let变量代替var，直接形成一个for为主体的块级作用域
* 此时i会到for里面找，找到对应的值
*/
for (let i = 0; i < btns.length; i ++) {
  btns[i].onclick = function () {
    console.log(i)
  }
}
```

temporal dead zone

如果在块级作用域中，用const和let声明变量，除非已经被初始化了，否则不能被访问

```javascript
var foo = "foo"

if (true) {
	console.log(foo)
	
	let foo = "123"
}
```

### 10.7 let，const，var的选择

var 不推荐使用，很容易出现bug，比如作用域提升等很奇怪的特性

尽量使用const来声明变量，如果之后要修改变量，再改成let

这是为了保证变量的安全性，不让externel随意修改变量

### 10.8 模版字符串 (*template literals*)

```javascript
const template_hello_world = `Hello Wolrd ${name}!`
```

标签模版字符串是可以调用函数的

```javascript
/**
* styled-components
*/
function foo(a, b) {
	console.log(a, b)
}

const name = "name"
// 第一个参数：['hello', 'wolrd']，被切成了多块，放到了数组中
// 第二个参数：$变量
foo`hello${name}world`
```

### 10.9 函数默认值

```javascript
// ES6
function foo(m=1, n=2) {
	console.log(m,n)
}
foo()
--------------------------
// ES5
function foo() {
	var m = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 1
	var n = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : 2
	console.log(m, n)
}
```

有默认值的话，会改变function.length的大小，因为从默认值之后开始的就不会计算length了

### 10.10 函数剩余参数

```javascript
/**
* args是一个数组
*/
function (a, b, ...args) {
	console.log(a, b)
	console.log(args)
}
```

Arguments 本质上不是数组，但是rest parameters是一个数组，同时必须放到参数列表的最后

### 10.11 展开运算符

```javascript
const names = ['abs', 'ss', 'gf']
const ages = [18, 10]
function (...names) {
	console.log(names)
}
const person = [...names, ...ages]
const obj = {...info, address: 'beijing', ...names}
```

浅拷贝

```javascript
const info = {
	name: 'rzh',
	age: 19,
  friend: {name: 'aa'}
}

const obj = {...info, name: 'abc'}

// 改变obj的friend name同时也会改变info的friend name，内存表现是一个地址存储了info，同时friend是另外一个地址。obj是直接将info内容复制到了一个新的地址中，但是引用的friend并没有改变，所以改变obj会改变info，因为指向同一个地址
// 深拷贝 JSON.stringify() + JSON.parse()
```

### 10.12 **Symbol (基本数据类型)

- 对象的属性都是以字符串形式表示的，容易引起冲突。如果不知道对象内部有哪些属性，直接添加会覆盖key（第三方框架）

```javascript
var obj = {
	name: 'rzh',
	friend: {name: 'ss'}
}
```

- 开发中使用了混入，如果出现同名属性，会被覆盖

**Symbol是一个函数，每个Symbol都是唯一的，即使内容一致也不会重复。**

生成后作为对象的属性key值

```javascript
const name = Symbol('name')
const age = Symbol('age')

const obj = {
    [name]: 'rzh',
    [age]: 18
}

obj[name] = 'rzh'

Object.defineProperty(obj, 'name', {
    enumerable: true,
    configurable: true,
    writable: true,
    value: 'ss'
})

console.log(obj[name])
```

Object.keys()直接获取keys，是获取不到Symbol的值的

**Object.getOwnPropertySymbols()**

Symbols 如果想要相同：Symbol.for()如果descriptor是相同的情况下，两者是相同的

获取key：Symbols.keyfor()

### 10.13 **Set

#### 10.13.1 Set

Es6之前的数据存储方式： Array和Object

ES6之后的数据存储：Set，Map，以及另外的形式WeakSet，WeakMap

set和其他language的set一致，元素不能重复

```javascript
const set = new Set()
set.add(10)
set.add(100)
set.add(10)
console.log(set) //Set(2) { 10, 100 }

const set = new Set()
set.add(10)
set.add({name: 'rzh'})
set.add({name: 'rzh'})
console.log(set) // Set(2) { {name: 'rzh'}, {name: 'rzh'}}

// 数组去重
const nums = [11, 22, 33, 44, 11, 11]

const set_1 = new Set()
for (num of nums) {
    set_1.add(num)
}

const new_nums = [...set_1]
const new_nums_from_array = Array.from(set_1)
-----------------------------------------------
const set_2 = new Set(nums)
const new_nums = [...set_1]
const new_nums_from_array = Array.from(set_1)
```

- add
- delete
- clear
- Has

Set的遍历

- ForEach，类似数组
- for of

#### 10.13.2 WeakSet（弱引用，GC回收）

Weakset中只能存放对象类型

Weakset对对象是弱引用，如果没有引用，会被GC回收

strong reference：回收时，引用是有效的

Weak reference：回收时，引用是没有用的，照样会被回收，但是其他和普通的引用一致

```javascript
const set = new Set()
const weak_set = new WeakSet()
let _obj = {
    name: 'rzh'
}

set.add(_obj)
weak_set.add(_obj)
```

分析上面的代码：

**_obj => 0x100{name: rzh} <= set(weak_set)，这时如果 _obj = null, 那么0x100的引用剩下了 set(weak_set) => 0x100，**

**set的引用是强引用，所以0x100不会被回收，但是weakset会被回收**

- has
- add
- delete

Weakset无法遍历，存储到weakSet中的对象无法获取

```javascript
const person_set = new WeakSet()
class Person {
    constructor() {
        person_set.add(this)
    }
    running() {
        if (!person_set.has(this)) {
            throw new Error('Cannot use running')
        }
        console.log('running')
    }
}
/**
* 如果销毁person，p = null
* 如果是set，则Person class不会被回收，因为person_set还引用着Person
*/
var p = new Person()
p.running()

p.running.call({name: 'ss'})
```

### 10.14 **Map

#### 10.14.1 Map

允许对象类型等其他数据类型作为key。

```javascript
const obj = {
    name: 'rzh'
}
const map = new Map()
map.set(obj, 'aa')
console.log(map) // Map(1) { { name: 'rzh' } => 'aa' }

const map_1 = new Map([[obj, 'aa']])
console.log(map_1) // Map(1) { { name: 'rzh' } => 'aa' }

map_1.get(obj)
map_1.has(obj)
map_1.delete(obj)

// 遍历
map.forEach(() => {})
for of
```

#### 10.14.2 WeakMap （响应式原理）

WeakMap的key只能是对象，不能接受其他类型

WeakMap是弱引用，会被GC回收

```javascript
let obj = {
	name: 'rzh'
}
const map = new Map([[obj, 'aa']])
const weak_map = new WeakMap([[obj, 'aa']])
obj = null
console.log(weak_map)
console.log(map)
// WeakMap { <items unknown> } obj的空间会被回收
// Map(1) { { name: 'rzh' } => 'aa' } obj的空间不会被回收
```

WeakMap针对响应式原理

```javascript
const obj = {
	name: 'rzh',
	age: 18
}

const obj_fun1 = () => {
	console.log('aa')
}

const obj_fun2 = () => {
	console.log('aa')
}

const obj_fun3 = () => {
	console.log('aa')
}

const obj_fun4 = () => {
	console.log('aa')
}

const map = new Map()
map.set('name', [obj_fun1, obj_fun2, obj_fun3, obj_fun4])
map.set('age', [obj_fun1, obj_fun2, obj_fun3, obj_fun4])
/**
* 使用weakMap的原因是如果obj销毁了，能直接将obj回收
*/
const weak_map = new WeakMap()
weak_map.set(obj1, map)

Object.defineProperties(obj, 'name', {
  enumerable: true,
  configurable: true,
  get() {
    return obj.name
  },
  set(new_val) {
    weak_map.get(obj1).get('name').forEach((item) => {
      item()
    })
  }
})
```

## 11. ES6+语法

### 11.1 Array.includes (ES7)

```javascript
const arr = [1, 2, 3]
arr.includes(1) // true false boolean值

arr.indexof(1) // 0 下标值
```

### 11.2 平方运算符 (ES7)

```javascript
Math.pow(3, 3)
or
3 ** 3
```

### 11.3 Object.values (ES8)

```javascript
var obj = {
	name: 'rzh',
	age: 18
}

console.log(Object.values(obj))

// 如果是数组
Object.values([1, 2, 3]) // 1, 2, 3
// 如果是字符串
Object.values('123') // 1, 2, 3
```

### 11.4 Object.entries (ES8)

```javascript
// 键值对数组
var obj = {
	name: 'rzh',
	age: 18
}
Object.entries(obj) // [ [ 'name', 'rzh' ], [ 'age', 18 ] ]

// 如果是数组，那么索引值作为key
console.log(Object.entries(arr)) // [ [ '0', 1 ], [ '1', 2 ], [ '2', 3 ] ]
```

### 11.5 String padding (ES8)

```javascript
const message = "Hello World"

/**
*	从开头开始填充 padStart
* 从最后开始填充 padENd
* 填充到指定长度
*/
const new_message = message.padStart(15, '-').padEnd(20, '+')
console.log(new_message) //----Hello World+++++
```

### 11.6 Object.getOwnPropertyDescriptors (ES8)

### 11.7 flat flatMap (ES10)

flat能够根据指定的深度遍历数组，然后将所有元素与遍历到的子数组中的元素合并成一个数组返回

```javascript
const nums = [1, 2, 3, [[123, 43], [12, 33], [[123, 22], [22]]], 4, [12, 56]]

console.log(nums.flat(3)) // 降3维
```

flatMap首先使用映射函数映射每个元素，然后将结果压缩成一个数组

和普通的map相比，自动降了一维

```javascript
const messages = ['hello world', 'hello 123', 'sdsa ss']

const words = messages.flatMap(item => {
	return item.split(" ")
})
```

### 11.8 Object.fromEntries (ES10)

```javascript
const entry = [[1, 2], [3, 4], [5, 6]]

Object.fromEntries(entry) // { '1': 2, '3': 4, '5': 6 }
```

### 11.9 trimStart, trimEnd (ES10)

去除字符串首部或者尾部的空格

### 11.10 BigInt (ES11)

在不使用BigInt的情况下，有些无法正确的表示int

**Number.MAX_SAFE_INTEGER**

```javascript
const maxInt = Number.MAX_SAFE_INTEGER
const bigInt = 900719925474099100n
// 加减乘除运算必须将数字转成大数
console.log(bigInt + 10n)

const smallNum = Number(bigInt)
```

### 11.11 optional chain (ES11)

```javascript
const info = {
    name: 'rzh',
    friend: {
        name: 'lil',
        friend: {
            name: 'rs'
        }
    }
}
// 如果是undefined，直接就不执行代码了
console.log(info.friend?.friend?.name)
```

### 11.12 globalThis全局对象 (ES11)

在浏览器中毫无疑问是window，在node中是global

globalThis这个包可以自动识别environment

### 11.13 FinalizationRegistry (ES12)

可以监听对象的回收情况，注册对象

GC是不定时的回收对象的，所以即使设置为null，也不会立马打印"done"，需要等待GC进行回收

```javascript
// FinalizationRegistry
const finalization_registry = new FinalizationRegistry((val) => {
    console.log(val + ' is done')
})

let person = {
    name: '123'
}

finalization_registry.register(person, "person")

person = null
```

### 11.14 WeakRef (ES12)

弱引用，可以传入一个对象，创建一个弱引用

```javascript
const finalization_registry = new FinalizationRegistry(() => {
    console.log('done')
})

let person = {
    name: '123'
}
let person_1 = new WeakRef(person)

finalization_registry.register(person)

person = null
```

### 11.15 逻辑赋值运算 Logical Assignment (ES12)

```javascript
let message ||= "default"
let obj &&= obj.name

```

## 12. Proxy, Reflect and 响应式原理

### 12.1 Object.defineProperty

使用Object.defineProperty + forEach 给所有的属性添加set和get方法

```javascript
/**
 * Object.defineProperty
 */
var obj = {
    name: '123',
    age: 18
}

Object.keys(obj).forEach(key => {
    let val = obj[key]
    Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        get() {
            console.log('Get...')
            return val
        },
        set(new_val) {
            console.log('Set..')
            val = new_val
        }
    })
})

obj.name = 'r'
obj.age = 20
console.log(obj.name)
```

缺点：

- 无法监听新增和删除属性

### 12.2 Proxy类

Proxy是一个类，可以使用new Proxy创建代理对象

对原对象的操作，全部在代理对象上面完成（捕获器 trap）

```javascript
/**
 * Proxy
 */
const o_proxy = new Proxy(obj, {
    get(target, key) {
        console.log('get from Proxy')
        return target[key]
    },
    set(target, key, val) {
        console.log('set from Proxy')
        target[key] = val
    }
})
o_proxy.name = "rzh"

console.log(o_proxy.name)
console.log(obj.name)
```

Set有四个参数：

target, key, val, receiver

get有三个参数：

Target, key, receiver

其他的一些trap：

```javascript
// 监听in的trap
has(target, key) {
console.log('in')
return key in target
},
// 删除
deleteProperty(target, key) {
console.log('delete')
delete target[key]
}
// getPrototypeOf
// setPrototypeOf
// ownKeys() getOwnPropertyNames getOwnPropertySymbols
// apply 函数对象
// constructor 函数对象

function foo() {
    
}
const foo_proxy = new Proxy(foo, {
    apply(target, thisArg, argArray) {
        console.log('apply')
        target.apply(thisArg)
    },
    construct(target, argArray) {
        console.log('constructor')
        return new target(argArray)
    }
})
foo_proxy.apply(obj)
new foo_proxy()
```

### 12.3 Reflect对象

提供了很多操作javascript的方法，类似于object的一系列方法

Reflect.defineProperty => Object.defineProperty

Reflect.getPrototypeOf => Object.getPrototypeOf

为了减轻Object的负担，将很多方法迁移到Reflect上

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods

Reflect的方法和Proxy提供的方法是完全一致的，所以可以避免在Proxy内部直接操作原对象

```javascript
const obj = {
    name: 'rzh',
    age: 18
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        console.log('get')
        return Reflect.get(target, key, receiver)
    },
    set(target, key, val, receiver) {
        console.log('set')
        Reflect.set(target, key, val, receiver)
    }
})

objProxy.name = "ff"
console.log(objProxy.name)
console.log(obj)
```

recevier是代理对象，考虑一个场景：

```javascript
const obj = {
    _name: 'rzh',
    get name() {
        return this._name
    },
    set name(new_val) {
        this._name = new_val
    }
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        Reflect.get(target, key, receiver)
    },
    set(target, key, val, receiver) {
        Reflect.set(target, key, val)
    }
})
```

使用Reflect.get的时候，会进入原obj的getter内部，调用this._name. 这里的this是obj，所以直接对原对象的私有 _name进行了操作，绕过了Proxy的机制。

如果同时传递了recevier，receiver会修改getter中this的指向，指向为proxy对象，这样会再次进入到get函数中，对_name进行拦截并且执行side effect

### 12.4 **响应式原理 (reactivity)

1. 首先定义一个函数收集依赖，所有需要响应式的函数作为参数传入到这个函数中。

```javascript
function watchEffect(fn) {
	
}

fn()
```

2. 定义一个subscriber和publisher，用一个class来区分不同的属性

```javascript
class Dependency {
    constructor() {
        this.reactiveFns = []
    }

    addDependency(reactiveFn) {
        this.reactiveFns.push(reactiveFn)
    }

    notify() {
        this.reactiveFns.forEach(fn => {
            fn()
        })
    }
}
```

3. 监听对象的属性变化

```javascript
const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        return Reflect.get(target, key)
    },
    set(target, key, val, receiver) {
        Reflect.set(target, key, val)
        depend.notify()
    }
})
```

4. 对不同属性进行不同的dependency区分

```javascript
/**
 * 为不同的对象创建Map结构
 */
const dependency_map = new WeakMap()
function getDepend(target, key) {
    // get target obj
    let map = dependency_map.get(target)
    if (! map) {
        map = new Map()
        dependency_map.set(target, map)
    }
    // get dependency by key
    let dependency = map.get(key)
    if (! dependency) {
        dependency = new Dependency()
        map.set(key, dependency)
    }
    return dependency
}
```

5. 重新处理收集依赖的逻辑

```javascript
// 全局的reactiveFn对象可以让Proxy获取到fn
let activeReactiveFn = null
function watchFn(fn) {
    // 执行fn之后，到了get方法之后可以拿到depend
    activeReactiveFn = fn
    fn()
    activeReactiveFn = null
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        const depend = getDepend(target, key)
        depend.addDependency(activeReactiveFn)
        return Reflect.get(target, key)
    },
```

6. 将普通的对象变成响应式的对象

```javascript
function reactive(obj) {
    return new Proxy(obj, {
        get(target, key, receiver) {
            const depend = getDepend(target, key)
            depend.addDependency(activeReactiveFn)
            return Reflect.get(target, key)
        },
        set(target, key, val, receiver) {
            Reflect.set(target, key, val)
            const depend = getDepend(target, key)
            depend.notify()
        }
    })
}
```

完整代码：

```javascript
class Dependency {
    constructor() {
        this.reactiveFns = []
    }

    addDependency(fn) {
        this.reactiveFns.push(fn)
    }

    notify() {
        this.reactiveFns.forEach((fn) => {
            fn()
        })
    }
}
const data_dependency = new WeakMap()
function getDepend(target, key) {
    let map = data_dependency.get(target)
    if (!map) {
        map = new Map()
        data_dependency.set(target, map)
    }
    let dependency = map.get(key)
    if (! dependency) {
        dependency = new Dependency()
        map.set(key, dependency)
    }
    return dependency
}
function reactive(obj) {
    return new Proxy(obj, {
        get(target, key, receiver) {
            const depend = getDepend(target, key)
            depend.addDependency(activeReactiveFn)
            return Reflect.get(target, key)
        },
        set(target, key, val, receiver) {
            Reflect.set(target, key, val)
            const depend = getDepend(target, key)
            depend.notify()
        }
    })
}
function reactive_by_property(obj) {
    Object.keys(obj).forEach(key => {
        let value = obj[key]
        Object.defineProperty(obj, key, {
            get() {
                const depend = getDepend(obj, key)
                depend.addDependency(activeReactiveFn)
                return value
            },
            set(new_val) {
                val = new_val
                const depend = getDepend(obj, key)
                depend.notify()
            }
        })
    })
    return obj
}
obj = {
    name: 'rzh',
    age: 18
}

const obj_proxy = reactive(obj)

let activeFn = null
function watchFn(fn) {
    activeFn = fn
    fn()
    activeFn = null
}

watchFn(function () {
    console.log(obj_proxy.name)
    console.log('hahaha')
})

watchFn(function () {
    console.log(obj_proxy.age)
    console.log('xixixi')
})

obj_proxy.name = 'foo'
obj_proxy.age = 20

/**
* output
*/
rzh
hahaha
18
xixixi
foo
hahaha
20
xixixi
```

## 13. **Promise Async function

### 13.1 类似Ajax的数据处理方式

类似ajax的网络请求

```javascript
function requestData(url) {
    setTimeout(() => {
        // 拿到请求的结果
        if (url == 'asd') {
            let nums = [1, 2, 3]
            return nums
        } else {
            let err = 'error'
            return err
        }
    }, 3000)
}

const res = requestData("asd")
```

拿不到res，因为return的值是在setTimeout这个函数中，并不是requestData这个函数的return值

ajax使用了callback function来解决

```javascript
function requestData(url, successCbFn, errorCbFn) {
    setTimeout(() => {
        // 拿到请求的结果
        if (url == 'asd') {
            let nums = [1, 2, 3]
            successCbFn(nums)
        } else {
            let err = 'error'
            errorCbFn(err)
        }
    }, 3000)
}

requestData("asd", (res) => {
    console.log(res)
}, (err) => {
    console.log(err)
})
```

缺点：

- 如果是第三方库，需要阅读source code，才能了解到requestData function的逻辑

### 13.2 Promise的改进方案

**Promise相当于自己封装了这一套逻辑，并且提供了standard的api来解决网络请求**

Promise接受回调函数，这个回调函数相当于在Promise的constructor中，所以会被立即执行，其中包含两个参数，resolve和reject

内部相当于：

```javascript
class Promise {
	constructor(callback) {
    	function resolve() {
        }
      	function reject() {
        }
      	
      	callback(resolve, reject)
    }
}
```

then方法传入的回调函数，会在Promise执行resolve函数时，被回调;

catch方法传入的回调函数，会在Promise执行reject函数时，被回调

```javascript
promise.then(() => {})
promise.catch(() => {})
```

使用Promise改进的代码

```javascript
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url == 'url') {
                const nums = [1, 2, 3]
                resolve(nums)
            } else {
                const errMessage = 'error'
                reject(errMessage)
            }
        }, 3000)
    })
}

const promise = requestData('url')
promise.then((nums) => {
    console.log(nums)
}).catch(err => {
    console.log(err)
})
```

### 13. 3 Promise的三个阶段

-- pending: request data

-- fulfilled: Get data

-- rejected: fail to get data

reject和resolve两个函数是互斥的，所以只能是fulfilled或者rejected两种状态

### 13.4 Promise的resolve参数

- Promise的resolve参数

如果resolve中的参数是一个Promise，那么当前的Promise的状态会由传入的Promise决定

传入的是一个对象，并且这个对象实现了then方法的话，那么会调用对象的then方法

### 13.5 Promise的then方法

那么其实Promise内部对于then和resolve的关联就应该是：

```javascript
class Promise {
	constructor(executor) {
    function resolve() {
      this.callback()
    }
    
    function reject() {
      
    }
    
    executor(resolve, reject)
  }
  
  then(callback) {
    this.callback = callback
  }
}
```

使用一个变量将callback保存，到resolve方法中执行

如果then方法返回一个值，这个值会被作为参数传递到一个新的Promise的resolve中作为回调

```javascript
new Promise((resolve, reject) => {
    resolve()
}).then(res => {
    return 'aaa'
}).then(res => {
    console.log(res) // aaa
})

// promise 链式调用，返回promise就代替之前的promise
new Promise((resolve, reject) => {
    resolve()
}).then(res => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('hello world')
        }, 3000)
    })
}).then(res => {
    console.log(res) // hello world
})
```

### 13.6 Promise的catch方法

catch方法可以捕获reject和throw出来的Error

```javascript
new Promise((resolve, reject) => {
  return new Promise((resolve, reject) => {
    reject('error')
    // throw new Error('123')
  })
}).catch(res => {
  console.log(res)
})

// 在catch中返回一个值，会new promise中调用resolve，所以还是会调用then方法
new Promise((resolve, reject) => {
  reject('111')
}).catch(res => {
  return 'new error'
}).then(res => {
  console.log(res)
}).catch(res => {
  console.log(res)
})
```

### 13.7 Promise的finally方法

finally方法在Promise调用完之后执行一些清理工作

