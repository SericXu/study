- ### Map和Set的区别
  >Set类似于数组，但是它里面每一项的值是唯一的，没有重复的值，Set是一个构造函数，用来生成set的数据结构。
  一般需要对多个数组求并集、差集、交集、去重等都会用到Set。

  常用方法:

    `add()`添加，返回 Set 结构本身（可以链式调用）

    `delete()`删除，删除成功返回true，否则返回false

    `has()`返回一个布尔值，表示该值是否为Set的成员

    `clear()`清除所有成员，没有返回值。

    `size()`长度

  >Map对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。构造函数Map可以接受一个数组作为参数。

  Map类似于对象，也是键值对的集合，但是“键”的范围不限制于字符串，各种类型的值（包含对象）都可以当作键。Map 也可以接受一个数组作为参数，数组的成员是一个个表示键值对的数组。注意Map里面也不可以放重复的项。

  常用方法:

  `set()`添加

  `delete()`删除

  `clear()`清楚所有

  `toString()`返回映射的字符串表示形式

  `valueOf()`返回指定对象的原始值

  `has()`如果映射包含指定元素，则返回 true

  `forEach()`遍历集合中对每个元素

  `keys()`返回键名的遍历器

  `values()`返回键值的遍历器

  `entries()`：返回键值对的遍历器

  #### ***主要有一下几个区别：***
  1.Map是键值对，Set是值的集合，当然键和值可以是任何的值；

  2.Map可以通过get方法获取值，而set不能因为它只有值；

  3.都能通过迭代器进行for…of遍历；

  4.Set的值是唯一的可以做数组去重，Map由于没有格式限制，可以做数据存储

  5.map和set都是stl中的关联容器，map以键值对的形式存储，key=value组成pair，是一组映射关系。set只有值，可以认为只有一个数据，并且set中元素不可以重复且自动排序。


- ### Map和Object的区别

>- 一个Object 的键只能是字符串或者 Symbols，但一个Map 的键可以是任意值。
-Map中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
-Map的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
-Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

----
- ### 数组的filter、every、flat的作用是什么


----
- ### es6有哪些新特性，前端开发者不得不知道的ES6十大特性


<details>
  <summary>
    <h4>Default Parameters（默认参数） in ES6</h3>
  </summary>
  在实际项目中，我们经常遇到需要函数传参对情况
  ```javascript
  function foo(bar){
    // bar 为一个obejct类型
    bar.name = 'zhangsan' //当bar为空时，此处会报错，程序将会终止
  }

  foo() // Error
  ```
  以上情形导致我们在调用`foo()`方法时必须保证参数不能为空才能保证程序正常运行。

  但现在ES6提供了默认参数特性可以解决这一问题：
  ```javascript
  function foo(bar = {}){
    // bar = {}， 在bar为空时，会默认给bar赋值{}
    bar.name = 'zhangsan' // 此处不会出错，正常运行
    console.log(bar.name)
  }
  foo() // 输出："zhangsan"
  ```
</details>
<details>
  <summary>
    <h4>Template Literals （模板文本）in ES6</h4>
  </summary>


  以前拼接字符串经常是这么做：
  ```javascript
  let str = "hello " + name
  ```
  现在：
  ```javascript
  let str = `hello ${name}`
  ```
  通过反引号"\`\`" + ${变量}来完成字符串拼接，其中${}可写表达式：
  ```javascript
  let str = `hello ${1 > 0 ? 'foo' : 'bar'}` // foo
  ```
</details>

<details>
  <summary>
    <h4>Multi-line Strings （多行字符串）in ES6</h4>
  </summary>

  ES5:
  ```javascript
  var roadPoem = 'Then took the other, as just as fair,nt'
    + 'And having perhaps the better claimnt'
    + 'Because it was grassy and wanted wear,nt'
    + 'Though as for that the passing therent'
    + 'Had worn them really about the same,nt';
var fourAgreements = 'You have the right to be you.n
    You can only be you when you do your best.';
  ```

  ES6:
  ```javascript
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim
    Because it was grassy and wanted wear,
    Though as for that the passing there
    Had worn them really about the same,`;
var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`;
  ```
</details>
<details>
  <summary>
    <h4>Destructuring Assignment （解构赋值）in ES6</h4>
  </summary>
  以前我们获取一个对象的属性可以这么写：
  ```javascript
  var obj = {
    name: 'zhangsan',
    age: 18
  }

  var name = obj.name
  var age = obj.age
  ...

  ```
  当获取一两个属性的时候这么写写也没什么问题，但是当有十几二十个属性的时候，需要写很多行代码，看起来十分冗余，且不利于维护，而ES6的解构赋值就完美的解决了这一问题：
  ```javascript
  let obj = {
    name: 'zhangsan',
    age: 18
  }
  let { name, age } = obj
  console.log(name, age) // zhangsan, 18
  ```

  我们可以利用这一特性完成很多复杂的操作，比如交换赋值。当需要给两个变量a,b交换一下各自的值的时候，以前都是需要一个新的临时变量c来作为中间的转换媒介：
  ```javascript
  let a = 1, b = 2
  let c = a // 保存a的值
  a = b // 将b的值给a
  b = c // 将c中保存的a的值给b
  ```
  当用结构赋值这一特性就变成了这样：
  ```javascript
  let a = 1, b =2
  [a, b] = [b, a] // a = 2, b = 1
  ```
  只需要一行代码，且不需要定义一个临时变量就可以完成
</details>

<details>
  <summary>
    <h4>Enhanced Object Literals （增强的对象文本）in ES6</h4>
  </summary>

> 相对于es5的简化写法

* 函数类属性的省略语法
```javascript
const obj = {
    //Before
    foo: function(){
        return 'foo';
    },

    //After
    bar(){
        return 'bar';
    }
}
```

* 支持 proto 注入

直接向一个对象字面量注入__proto__，使其直接成为指定类的一个实例，而无须另外创建一个类来实现继承。

```javascript
import {EventEmitter} from 'events'

const machine = {
    __proto__: new EventEmitter(),

    method(){},
    ...
}

console.log(machine);  //EventEmitter {}
console.log(machine instanceof EventEmitter)  //true

machine.on('event', msg => console.log(`Received message: ${msg}`));
machine.emit('event', 'hello world');
// Received message: hello world

machine.method(/* …. */);
```

* 可动态计算的属性名
使用一个表达式来表达一个属性名：`{ [statement]: value}`
```javascript
const prefix = 'ES6';
const obj = {
    [prefix + 'enhancedObject']: 'foo'
}
```
* 将属性名定义省略
变量名和属性名都是相同的，可以对属性名定义进行省略
```javascript
const foo = 123;
const bar = () => foo;
const obj = {
    foo,
    bar
}
console.log(obj);  //{ foo: 123, bar: [Function] }
```
</details>

<details>
  <summary>
    <h4>Arrow Functions （箭头函数）in ES6</h4>
  </summary>
箭头函数和普通函数的区别：
- 声明方式不同，匿名函数

  箭头函数只需要`() => `，普通函数需要声明Function关键字
- <strong style="color: red">this指向不同</strong>
> 普通函数的this指向运行时调用该方法时所在的上下文对象，箭头函数<span style="color: red">没有自己的this对像</span>，内部的this就是定义时上层作用域中的this。也就是说，***箭头函数内部的this指向是固定的，相比之下，普通函数的this指向是可变的***。

  ```javascript
  var name = 'foo1'
  var person = {
      name: 'foo2',
      say: function() {
          console.log('say:',this.name)
      },
      say2: () => {
          console.log('say2:',this.name)
      }
  }
  person.say() // say: foo2
  person.say2() // say2: foo1
  ```
  这里第一个`say`定义的是一个普通函数，并且它是作为对象`person`的方法来进行调用的，所以它的`this`指向的就是`person`，所以它应该会输出`say: foo2`
而第二个`say2`定义的是一个箭头函数，我们知道箭头函数本身没有this，它的this永远指向它定义时所在的上层作用域，所以`say2`的`this`应该指向的是全局`window`，所以它会输出`say2: foo1`
我们也可以通过Babel 转箭头函数产生的 ES5 代码来证明箭头函数没有自己的this，而是引用的上层作用域中this。

  ```javascript
  // ES6
  function foo() {
    setTimeout(() => {
      console.log('id:', this.id); // 此处的this是foo中的this
    }, 100);
  }

  // ES5
  function foo() {
    var _this = this;

    setTimeout(function () {
      console.log('id:', _this.id);
    }, 100);
  }
  ```

* <strong style="color: red">箭头函数的this永远不会变，call、apply、bind也无法改变</strong>
* 箭头函数没有原型prototype
* 箭头函数不能当成一个构造函数

  因为箭头函数没有自己的this，执行new的步骤：
  ```javascript
  function myNew() {
  // 1.新建一个空对象
  let obj = {}
  // 2.获得构造函数
  let con = arguments.__proto__.constructor
  // 3.链接原型
  obj.__proto__ = con.prototype
  // 4.绑定this，执行构造函数
  let res = con.apply(obj, arguments)
  // 5.返回新对象
  return typeof res === 'object' ? res : obj
  }
  ```
  它的`this`其实是继承了外层执行环境中的`this`，且`this`指向***永远不会变***，并且箭头函数没有原型`prototype`，没法让他的实例的`__proto__`属性指向，所以箭头函数也就无法作为构造函数，否则用new调用时会报错！

* 没有new.target，箭头函数没有自己的arguments，箭头函数处于全局作用域中，则没有arguments

</details>

<details>
  <summary>
    <h4>Promises in ES6</h4>
  </summary>
  
</details>

<details>
  <summary>
    <h4>Block-Scoped Constructs Let and Const（块作用域构造 Let and Const）</h4>
  </summary>

</details>

<details>
  <summary>
    <h4>Classes（类） in ES6</h4>
  </summary>

</details>

<details>
  <summary>
    <h4>Modules（模块） in ES6</h4>
  </summary>

</details>


----
- Promise实现原理


----
- `Promise.all()`和`Promise.race()`有什么区别？


----
- 箭头函数和普通函数的区别


----
- let、var、const的区别？const定义的对象，使其属性不能被修改怎么办？


----
- 堆和栈的区别


----
- 闭包的原理


----
- instanceof的实现原理


----
- new的实现原理


----
- 数据类型有哪些？如何判断一个数据是否是数组


----
- JQuery实现链式调用的原理是什么


----
- 分别介绍一下原型、原型链、作用域和作用域链的含义和使用场景
[链接名称](链接地址)
