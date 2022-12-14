# 设计模式

[TOC]

## UML 类图

画出类的内容，继承等

常见的有以下几种关系: 泛化（Generalization）, 实现（Realization），关联（Association)，聚合（Aggregation），组合(Composition)，依赖(Dependency)，例如

```bash
-表示private
#表示protected
~表示default,也就是包权限
_下划线表示static
斜体表示抽象
```

## 设计原则

1. 准则 1：小既是美
2. 准则 2：让每个程序只做好一件事
3. 准则 3：快速建立原型
4. 准则 4：舍弃高效率取可移植性
5. 准则 5：采用纯文本来存储数据
6. 准则 6：软件复用
7. 准则 7：使用 shell 脚本提高杠杆效应和可移植性
8. 准则 8：避免强制性用户界面
9. 准则 9：让每个程序称为过滤器

### 小准则

1. 允许用户定制文件
2. 尽量使操作系统内核小而轻量化
3. 使用小写字母并且尽量简短
4. 沉默是金
5. 各部分之和大于整体
6. 寻求 90%解决方案

### 5 大设计原则（SOLID）

1. S：单一职责原则，一个程序只做好一个事
2. O：开放封闭原则，对扩展开放对修改封闭
3. L：李氏置换原则，子类能覆盖父类
4. I：接口独立原则，保持接口独立
5. D：依赖导致原则，只关注接口不关注具体实现

## 模式

设计到模式，总结出来固定的东西

## 23 种设计模式

### 创建型

1. 工厂模式
2. 单例模式
3. 原型模式

### 结构型

1. 适配器模式
2. 装饰器模式
3. 代理模式
4. 外观模式
5. 桥接模式
6. 组合模式
7. 享元模式

### 行为型

1. 策略模式
2. 模板方法模式
3. 观察者模式
4. 迭代器模式
5. 职责连模式
6. 命令模式
7. 备忘录模式
8. 状态模式
9. 访问者模式
10. 中介者模式
11. 解释器模式

## 面试题

### **第一**

打车时车都有车牌号和名称，打专车或者快车，快车每公里 1 元，专车每公里 2 元，开车时显示车辆信息，结束时显示打车金额，假定行程 5 公里

**思路**

父类是车有车牌号等信息，子类有专车和快车不同价格，行程类与车有关系

### 第二

停车场，分 3 层，每层 100 车位，每个车位都能监控车辆驶入和离开。车进入前，显示每层空余车位数量；车辆进入后，摄像头识别车牌号和时间；车辆出来时，显示车牌号和停车时长

**思路**

父类停车，层，车位都是子类

## 工厂模式

单独封装，用一个接口实现一个具体的类(使用者不用管封装方法，只要调用接口就可以实现)

**意图：**定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。

**主要解决：**主要解决接口选择的问题。

**何时使用：**我们明确地计划不同条件下创建不同实例时。

**如何解决：**让其子类实现工厂接口，返回的也是一个抽象的产品。

**关键代码：**创建过程在其子类执行。

**应用实例：** 1、您需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。 2、Hibernate 换数据库只需换方言和驱动就可以。

1. **优点：** 1、一个调用者想创建一个对象，只要知道其名称就可以了。 2、扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。 3、屏蔽产品的具体实现，调用者只关心产品的接口。

2. **缺点：**每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

3. **使用场景：** jq=> $('div')，React.createElement，vue 异步组件
4. 构造方法和创建者分离，符合开放封闭原则

```js
class Product {
  constructor(name) {
    this.name = name;
  }
  init() {}
  fun1() {}
  fun2() {}
}

class Creator {
  create(name) {
    return new Product(name);
  }
}

let creator = new Creator();
let p = creator.create("f1");
p.init();
p.fun1();
```

## 单例模式

系统被唯一使用，一个类只有一个实例，例如，登录框，购物车，jq 只有一个$，vuex 和 redux 的 store

**设计原则** 符合单一职责原则

**意图：**保证一个类仅有一个实例，并提供一个访问它的全局访问点。

**主要解决：**一个全局使用的类频繁地创建与销毁。

**何时使用：**当您想控制实例数目，节省系统资源的时候。

**如何解决：**判断系统是否已经有这个单例，如果有则返回，如果没有则创建。

**关键代码：**构造函数是私有的。

**优点：**

- 1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。
- 2、避免对资源的多重占用（比如写文件操作）。

**缺点：**没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

**使用场景：**

- 1、要求生产唯一序列号。
- 2、WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。
- 3、创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。

```js
//使用对象属性也可以实现
class SingleObject {
  login() {
    console.log("...");
  }

  static getInstance = (() => {
    let instance;
    return function () {
      if (!instance) {
        instance = new SingleObject();
      }
      return instance;
    };
  })();
}

let obj1 = SingleObject.getInstance();
obj1.login();
let obj2 = SingleObject.getInstance();
obj2.login();

console.log(obj1 === obj2); //true
```

## 适配器模式

把旧的接口转换成需要被使用的合适接口，封装旧接口，vue computed

**原则**：符合开放封闭原则

```js
class Adaptee {
  specificRequest() {
    return "德国";
  }
}

class Target {
  constructor() {
    this.adaptee = new Adaptee();
  }

  request() {
    let info = this.adaptee.specificRequest();
    return `${info} -转换器`;
  }
}

let target = new Target();
let res = target.request();
```

## 装饰器模式

为对象添加新功能，不改变其原有的结构和功能，es7 装饰器，core-decoration

**原则**：将装饰器和对象分离独立存在，符合开放封闭原则

**意图：**动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。

**主要解决：**一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。

**何时使用：**在不想增加很多子类的情况下扩展类。

**如何解决：**将具体功能职责划分，同时继承装饰者模式。

**关键代码：** 1、Component 类充当抽象角色，不应该具体实现。 2、修饰类引用和继承 Component 类，具体扩展类重写父类方法。

**应用实例：** 1、孙悟空有 72 变，当他变成"庙宇"后，他的根本还是一只猴子，但是他又有了庙宇的功能。 2、不论一幅画有没有画框都可以挂在墙上，但是通常都是有画框的，并且实际上是画框被挂在墙上。在挂在墙上之前，画可以被蒙上玻璃，装到框子里；这时画、玻璃和画框形成了一个物体。

**优点：**装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。

**缺点：**多层装饰比较复杂。

```js
class Circle {
  draw() {
    console.log("画一个圆形");
  }
}
class Decorator {
  constructor(circle) {
    this.circle = circle;
  }
  draw() {
    this.circle.draw();
    this.setBorder(circle);
  }
  setBorder() {
    console.log("设置红色边框");
  }
}
```

## 代理模式

使用者无权访问目标对象，中间加代理做授权和控制，网页事件代理和 es6 的 proxy，jq 的$proxy

在代理模式（Proxy Pattern）中，一个类代表另一个类的功能。这种类型的设计模式属于结构型模式。

在代理模式中，我们创建具有现有对象的对象，以便向外界提供功能接口。

### 和适配器，装饰器的区别

1. 适配器提供不同的接口，代理模式提供一模一样的接口
2. 装饰器扩展功能原有功能不变，代理模式显示原有的功能，但是经过限制

**意图：**为其他对象提供一种代理以控制对这个对象的访问。

**主要解决：**在直接访问对象时带来的问题，比如说：要访问的对象在远程的机器上。在面向对象系统中，有些对象由于某些原因（比如对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问），直接访问会给使用者或者系统结构带来很多麻烦，我们可以在访问此对象时加上一个对此对象的访问层。

**何时使用：**想在访问一个类时做一些控制。

**如何解决：**增加中间层。

**关键代码：**实现与被代理类组合。

**应用实例：** 1、Windows 里面的快捷方式。 2、猪八戒去找高翠兰结果是孙悟空变的，可以这样理解：把高翠兰的外貌抽象出来，高翠兰本人和孙悟空都实现了这个接口，猪八戒访问高翠兰的时候看不出来这个是孙悟空，所以说孙悟空是高翠兰代理类。 3、买火车票不一定在火车站买，也可以去代售点。 4、一张支票或银行存单是账户中资金的代理。支票在市场交易中用来代替现金，并提供对签发人账号上资金的控制。 5、spring aop。

**优点：** 1、职责清晰。 2、高扩展性。 3、智能化。

**缺点：** 1、由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢。 2、实现代理模式需要额外的工作，有些代理模式的实现非常复杂。

```js
class ReadImg {
  constructor(fileName) {
    this.fileName = fileName;
    this.loadFormDisk();
  }

  display() {
    console.log(this.fileName);
  }
  loadFormDisk() {
    console.log("loading");
  }
}

class ProxyImg {
  constructor(fileName) {
    this.realImg = new ReadImg(fileName);
  }
  display() {
    this.realImg.display();
  }
}

let proxy = new ProxyImg("1.png");
proxy.display();
```

## 外观模式

为子系统的一组接口提供高层接口，使用者使用高层接口

## 观察者模式

发布订阅，一对多，网页事件绑定，Promise，jq callback，node 自定义，react 和 vue 的生命周期函数触发

**意图：**定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

**主要解决：**一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。

**何时使用：**一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。

**如何解决：**使用面向对象技术，可以将这种依赖关系弱化。

**关键代码：**在抽象类里有一个 ArrayList 存放观察者们。

**应用实例：** 1、拍卖的时候，拍卖师观察最高标价，然后通知给其他竞价者竞价。 2、西游记里面悟空请求菩萨降服红孩儿，菩萨洒了一地水招来一个老乌龟，这个乌龟就是观察者，他观察菩萨洒水这个动作。

**优点：** 1、观察者和被观察者是抽象耦合的。 2、建立一套触发机制。

**缺点：** 1、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 2、如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。 3、观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

### 和发布订阅的区别

- 观察者模式里，只有两个角色 —— 观察者 + 被观察者
- 而发布订阅模式里，却不仅仅只有发布者和订阅者两个角色，还有一个经常被我们忽略的 —— 经纪人 Broker

```js
class Subject {
  constructor() {
    this.state = 0;
    this.observers = [];
  }
  getState() {
    return this.state;
  }
  setState(state) {
    this.state = state;
    this.notifyAllObercers();
  }
  notifyAllObercers() {
    this.observers.forEach((item) => {
      item.update();
    });
  }
  attach(observer) {
    this.observers.push(observer);
  }
}

class Observer {
  constructor(name, subject) {
    this.name = name;
    this.subject = subject;
    this.subject.attach(this);
  }
  update() {
    console.log("update");
  }
}

let s = new Subject();
let o1 = new Observer("o1", s);
s.setState(1);
```

## 迭代器模式

顺序访问一个集合，使用者无需访问内部结构（封装），jq each，es6 Iterator

**原则**：目标对象和迭代器分开

## 状态模式

一个对象有状态变化，每次状态变化都触发一个逻辑，不能总是拿 if...else 控制

## 原型模式

clone 自己生成一个新的对象，Object.create

## 桥接模式

用于抽象化与实现化解耦，使得两者独立化

## 组合模式

生成树形结构表示整体和部分关系，让整体和部分具有一致操作方式

## 享元模式

共享内存，相同的数据共享使用

## 策略模式

不同策略分开处理，避免出现大量的 if..else

## 命令模式

发送者和接收者分开
