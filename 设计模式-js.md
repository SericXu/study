驾驭技术的能力

- 能用健壮的代码去解决具体的问题；
- 能用抽象的思维去应对复杂的系统；
- 能用工程化的思想去规划更大规模的业务。

第一章 设计模式的道与术

------

### SOLID设计原则

- 单一功能原则（Single Responsibility Principle）
- 开放封闭原则（Opened Closed Principle）
- 里式替换原则（Liskov Substitution Principle）
- 接口隔离原则（Interface Segregation Principle）
- 依赖反转原则（Dependency Inversion Principle）

单一功能、开放封闭为常用

### 设计模式的核心思想——==封装变化==

**将变与不变分离，确保变化的部分灵活、不变的部分稳定** **——** **设计模式的道**

***23种经典设计模式 —— 设计模式的术***

三大类型

- 创建型
- - 单例模式
  - 原型模式
  - 构造器模式
  - 工厂模式
  - 抽象工厂模式
- 结构型
- - 桥接模式
  - 外观模式
  - 组合模式
  - 装饰器模式
  - 适配器模式
  - 代理模式
  - 享元模式
- 行为型
- - 迭代器模式
  - 解释器模式
  - 观察者模式
  - 中介者模式
  - 访问者模式
  - 状态模式
  - 备忘录模式
  - 策略模式
  - 模板方法模式
  - 职责链模式
  - 命令模式

----





## 构造器模式

例子：

```js
const Bruce = {
  name: '布鲁斯',
  age: 25,
  career: 'coder'
}

// 构造器
// es6 class 等同于 es5 function
function User(name, age, career){
  this.name = name
  this.age = age
  this.career = career
}
```



以上的过程就是抽象了每个User实例的变与不变，不变的是User的name，age，career等共有的属性，但是每个属性的值是变化的，所以再次说明了设计模式是确保共性的不变，同时开放了可变性。

----





## 简单工厂模式

当上面的User需要根据career不同的值，分配不同的work时，构造器就不太好用了。一两个还好说，当有成千上万的User时，也不能手写同样多的类来解决。

**初步解决**：将判断人员工种，在构造器中交给函数去处理

```js
function Factory(name, age, career) {
    switch(career) {
        case 'coder':
            return new Coder(name, age)
            break
        case 'product manager':
            return new ProductManager(name, age)
            break
        ...
}
```

看起来是好一些了，至少我们不用操心构造函数的分配问题了。但是大家注意我在 switch 的末尾写了个省略号，这个省略号比较恐怖。看着这个省略号，李雷哭了，他想到：整个公司上下有数十个工种，难道我要手写数十个类、数十行 switch 吗？

***变的是什么？不变的又是什么？***

Coder 和 ProductManager 两个工种的员工，是不是仍然存在都拥有 name、age、career、work 这四个属性这样的共性？它们之间的区别，在于每个字段取值的不同，以及 work 字段需要随 career 字段取值的不同而改变。这样一来，我们是不是对共性封装得不够彻底？那么相应地，共性与个性是不是分离得也不够彻底？
现在我们把相同的逻辑封装回User类里，然后把这个承载了==共性的 User 类和个性化的逻辑判断==写入同一个函数：

```javascript
function User(name , age, career, work) {
    this.name = name
    this.age = age
    this.career = career
    this.work = work
}

function Factory(name, age, career) {
    let work
    switch(career) {
        case 'coder':
            work =  ['写代码','写系分', '修Bug']
            break
        case 'product manager':
            work = ['订会议室', '写PRD', '催更']
            break
        case 'boss':
            work = ['喝茶', '看报', '见客户']
        case 'xxx':
            // 其它工种的职责分配
            ...

    return new User(name, age, career, work)
}
```

这样一来，不用自己时刻想着拿到的这组数据是什么工种、怎么给它分配构造函数，更不用手写无数个构造函数——Factory已经帮我们做完了一切，而我们只需要像以前一样**无脑传参**就可以了！

总结：

***工厂模式***其实就是==将创建对象的过程单独封装==，我传参这个过程就是点菜，工厂函数里面运转的逻辑就相当于炒菜的厨师和上桌的服务员做掉的那部分工作——这部分工作我们同样不用关心，我们只要能拿到工厂交付给我们的实例结果就行了

工厂模式的目的就是为了实现无脑传参

小结：

应用场景也非常容易识别：有构造函数的地方，我们就应该想到简单工厂；在写了大量构造函数、调用了大量的 new、自觉非常不爽的情况下，我们就应该思考是不是可以掏出工厂模式重构我们的代码了。

----







## 抽象工厂模式

与简单工厂模式的共同点：

- **尝试去分离一个系统中变与不变的部分**

不同在于**==场景的复杂度==**

简单工厂的使用场景里，处理的对象是类，并且是一些非常好对付的类——它们的共性容易抽离，同时因为逻辑本身比较简单，故而不苛求代码可扩展性。抽象工厂本质上处理的其实也是类，但是是一帮非常棘手、繁杂的类，这些类中不仅能划分出门派，还能划分出等级，同时存在着千变万化的扩展可能性。

抽象工厂模式是对比较复杂的场景进行类的性质的划分，可分成四个：

- **抽象工厂（抽象类，用于约束，制定规范）**
- **具体工厂（继承自抽象工厂，实现抽象工厂的方法）**
- **抽象产品（抽象类）**
- **具体产品**



==抽象工厂模式的定义，是**围绕一个超级工厂创建其他工厂**。==

抽象工厂模式很好的解释了***开放封闭原则***



----







## 单例模式

> 保证仅有一个实例，并提供一个访问它的全局访问点。
>
> vuex、redux就是典型的单例模式的应用

例子：

```js
class SingleDog {
    show() {
        console.log('我是一个单例对象')
    }
    static getInstance() {
        // 判断是否已经new过1个实例
        if (!SingleDog.instance) {
            // 若这个唯一的实例不存在，那么先创建它
            SingleDog.instance = new SingleDog()
        }
        // 如果这个唯一的实例已经存在，则直接返回
        return SingleDog.instance
    }
}

const s1 = SingleDog.getInstance()
const s2 = SingleDog.getInstance()

// true
s1 === s2
```

> vuex源码中的install方法：

```js
let Vue // 这个Vue的作用和楼上的instance作用一样
...

export function install (_Vue) {
  // 判断传入的Vue实例对象是否已经被install过Vuex插件（是否有了唯一的state）
  if (Vue && _Vue === Vue) {
    if (process.env.NODE_ENV !== 'production') {
      console.error(
        '[vuex] already installed. Vue.use(Vuex) should be called only once.'
      )
    }
    return
  }
  // 若没有，则为这个Vue实例对象install一个唯一的Vuex
  Vue = _Vue
  // 将Vuex的初始化逻辑写进Vue的钩子函数里
  applyMixin(Vue)
}
```



> ### **面试题：实现一个Storage**

### 描述

> 实现Storage，使得该对象为单例，基于 localStorage 进行封装。实现方法 setItem(key,value) 和 getItem(key)。

### 思路

拿到单例模式相关的面试题，大家首先要做的是回忆我们上个小节的“基本思路”部分——至少要记起来`getInstance`方法和`instance`这个变量是干啥的。

具体实现上，把判断逻辑写入静态方法或者构造函数里都没关系，最好能把闭包的版本也写出来，多多益善。

总之有了上节的基础，这个题简直是默写！

> 实现：静态方法版

```javascript
// 定义Storage
class Storage {
    static getInstance() {
        // 判断是否已经new过1个实例
        if (!Storage.instance) {
            // 若这个唯一的实例不存在，那么先创建它
            Storage.instance = new Storage()
        }
        // 如果这个唯一的实例已经存在，则直接返回
        return Storage.instance
    }
    getItem (key) {
        return localStorage.getItem(key)
    }
    setItem (key, value) {
        return localStorage.setItem(key, value)
    }
}

const storage1 = Storage.getInstance()
const storage2 = Storage.getInstance()

storage1.setItem('name', '李雷')
// 李雷
storage1.getItem('name')
// 也是李雷
storage2.getItem('name')

// 返回true
storage1 === storage2
```

> 实现： 闭包版

```javascript
// 先实现一个基础的StorageBase类，把getItem和setItem方法放在它的原型链上
function StorageBase () {}
StorageBase.prototype.getItem = function (key){
    return localStorage.getItem(key)
}
StorageBase.prototype.setItem = function (key, value) {
    return localStorage.setItem(key, value)
}

// 以闭包的形式创建一个引用自由变量的构造函数
const Storage = (function(){
    let instance = null
    return function(){
        // 判断自由变量是否为null
        if(!instance) {
            // 如果为null则new出唯一实例
            instance = new StorageBase()
        }
        return instance
    }
})()

// 这里其实不用 new Storage 的形式调用，直接 Storage() 也会有一样的效果
const storage1 = new Storage()
const storage2 = new Storage()

storage1.setItem('name', '李雷')
// 李雷
storage1.getItem('name')
// 也是李雷
storage2.getItem('name')

// 返回true
storage1 === storage2
```



## 实现一个全局的模态框

### 描述

> 实现一个全局唯一的Modal弹框

### 思路

这道题比较经典，基本上所有讲单例模式的文章都会以此为例，同时它也是早期单例模式在前端领域的最集中体现。

万变不离其踪，记住`getInstance`方法、记住`instance`变量、记住闭包和静态方法，这个题除了要多写点 HTML 和 CSS 之外，对大家来说完全不成问题。

### 实现

完整代码如下：

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>单例模式弹框</title>
</head>
<style>
    #modal {
        height: 200px;
        width: 200px;
        line-height: 200px;
        position: fixed;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        border: 1px solid black;
        text-align: center;
    }
</style>
<body>
	<button id='open'>打开弹框</button>
	<button id='close'>关闭弹框</button>
</body>
<script>
    // 核心逻辑，这里采用了闭包思路来实现单例模式
    const Modal = (function() {
    	let modal = null
    	return function() {
            if(!modal) {
            	modal = document.createElement('div')
            	modal.innerHTML = '我是一个全局唯一的Modal'
            	modal.id = 'modal'
            	modal.style.display = 'none'
            	document.body.appendChild(modal)
            }
            return modal
    	}
    })()

    // 点击打开按钮展示模态框
    document.getElementById('open').addEventListener('click', function() {
        // 未点击则不创建modal实例，避免不必要的内存占用;此处不用 new Modal 的形式调用也可以，和 Storage 同理
    	const modal = new Modal()
    	modal.style.display = 'block'
    })

    // 点击关闭按钮隐藏模态框
    document.getElementById('close').addEventListener('click', function() {
    	const modal = new Modal()
    	if(modal) {
    	    modal.style.display = 'none'
    	}
    })
</script>
</html>
```



------







## 原型模式

> ####谈原型模式，其实是谈原型范式

原型编程范式的核心思想就是**利用实例来描述对象，用实例作为定义对象和继承的基础**。在 JavaScript 中，原型编程范式的体现就是**基于原型链的继承**。这其中，对原型、原型链的理解是关键。

对象的深拷贝：

- JSON.stringify()

  - 这个方法存在一些局限性，比如无法处理 function、无法处理正则等等——只有当你的对象是一个严格的 JSON 对象时，可以顺利使用这个方法

- **深拷贝没有完美方案，每一种方案都有它的边界 case**。而面试官向你发问也并非是要求你破解人类未解之谜，多数情况下，他只是希望考查你对**递归**的熟练程度。所以递归实现深拷贝的核心思路，大家需要重点掌握（解析在注释里）：

  ```js
  function deepClone(obj) {
      // 如果是 值类型 或 null，则直接return
      if(typeof obj !== 'object' || obj === null) {
          return obj
      }

      // 定义结果对象
      let copy = {}

      // 如果对象是数组，则定义结果数组
      if(obj.constructor === Array) {
          copy = []
      }

      // 遍历对象的key
      for(let key in obj) {
          // 如果key是对象的自有属性
          if(obj.hasOwnProperty(key)) {
              // 递归调用深拷贝方法
              copy[key] = deepClone(obj[key])
          }
      }

      return copy
  }
  ```



### 拓展阅读

深拷贝在命题时，可发挥的空间主要在于针对不同数据结构的处理，比如除了考虑 Array、Object，还需要考虑一些其它的数据结构（Map、Set 等）；此外还有一些极端 case（循环引用等）的处理等等。深拷贝的实现细节，这里为大家推荐两个阅读材料：

- [jQuery中的extend方法源码](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjquery%2Fjquery%2Fblob%2F1472290917f17af05e98007136096784f9051fab%2Fsrc%2Fcore.js%23L121)
- [深拷贝的终极探索](https://link.juejin.cn/?target=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000016672263)

----





> # 结构型



## 装饰器模式

在 ES7 中，我们可以像写 python 一样通过一个@语法糖轻松地给一个类装上装饰器：

```javascript
// 装饰器函数，它的第一个参数是目标类
function classDecorator(target) {
    target.hasDecorator = true
  	return target
}

// 将装饰器“安装”到Button类上
@classDecorator
class Button {
    // Button类的相关逻辑
}

// 验证装饰器是否生效
console.log('Button 是否被装饰了：', Button.hasDecorator)
```

> 类装饰器

```js
function classDecorator(target) {
    target.hasDecorator = true
  	return target
}

// 将装饰器“安装”到Button类上
@classDecorator
class Button {
    // Button类的相关逻辑
}
```

> 方法装饰器

```js
function funcDecorator(target, name, descriptor) {
    let originalMethod = descriptor.value
    descriptor.value = function() {
    console.log('我是Func的装饰器逻辑')
    return originalMethod.apply(this, arguments)
  }
  return descriptor
}

class Button {
    @funcDecorator
    onClick() {
        console.log('我是Func的原有逻辑')
    }
}
```

直接放进浏览器/Node 中运行会报错，因为浏览器和 Node 目前都不支持装饰器语法，需要大家安装 [Babel](https://link.juejin.cn/?target=https%3A%2F%2Fbabeljs.io%2F) 进行转码：

> 安装 Babel 及装饰器相关的 Babel 插件

```
npm install babel-preset-env babel-plugin-transform-decorators-legacy --save-dev
```

> 注：在没有任何配置选项的情况下，babel-preset-env 与 babel-preset-latest（或者 babel-preset-es2015，babel-preset-es2016 和 babel-preset-es2017 一起）的行为完全相同。
>
> > 编写配置文件.babelrc：
>
> ```json
> {
>   "presets": ["env"],
>   "plugins": ["transform-decorators-legacy"]
> }
> ```
>
> 最后别忘了下载全局的 Babel 命令行工具用于转码：
>
> ```
> npm install babel-cli -g
> ```
>
> 执行完这波操作，我们首先是对目标文件进行转码，比如说你的目标文件叫做 `test.js`，想要把它转码后的结果输出到 `babel_test.js`，就可以这么写:
>
> ```
> babel test.js --out-file babel_test.js
> ```
>
> > 运行babel_test.js
>
> ```
> babel_test.js
> ```
>
> 就可以看到你的装饰器是否生效啦~

装饰模式库 —— [core-decorators](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjayphelps%2Fcore-decorators)

----

## 适配器模式

> 为了兼容接口 -- 旧的业务需要拓展

例子：
fetch
```JavaScript
export default class HttpUtils {
  // get方法
  static get(url) {
    return new Promise((resolve, reject) => {
      // 调用fetch
      fetch(url)
        .then(response => response.json())
        .then(result => {
          resolve(result)
        })
        .catch(error => {
          reject(error)
        })
    })
  }

  // post方法，data以object形式传入
  static post(url, data) {
    return new Promise((resolve, reject) => {
      // 调用fetch
      fetch(url, {
        method: 'POST',
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/x-www-form-urlencoded'
        },
        // 将object类型的数据格式化为合法的body参数
        body: this.changeData(data)
      })
        .then(response => response.json())
        .then(result => {
          resolve(result)
        })
        .catch(error => {
          reject(error)
        })
    })
  }

  // body请求体的格式化方法
  static changeData(obj) {
    var prop,
      str = ''
    var i = 0
    for (prop in obj) {
      if (!prop) {
        return
      }
      if (i == 0) {
        str += prop + '=' + obj[prop]
      } else {
        str += '&' + prop + '=' + obj[prop]
      }
      i++
    }
    return str
  }
}
```
调用方式：
```javascript
// 定义目标url地址
const URL = "xxxxx"
// 定义post入参
const params = {
  ...
}

// 发起post请求
const postResponse = await HttpUtils.post(URL,params) || {}

// 发起get请求
const getResponse = await HttpUtils.get(URL) || {}
```
