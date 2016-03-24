
##深入读JavaScript面向对象编程

<br>面向对象编程是用抽象方式创建基于现实世界模型的一种编程模式，主要包括模块化、多态、和封装几种技术。而这些一般都是后台例如java有的思想。 对JavaScript而言，其核心是支持面向对象的，同时它也提供了强大灵活的基于原型的面向对象编程能力。 本文将会深入的探讨有关使用JavaScript进行面向对象编程的一些核心基础知识，包括对象的创建，继承机制， 最后还会简要的介绍如何借助ES6提供的新的类机制重写传统的JavaScript面向对象代码。
<br>
<br>
###面向对象的几个概念

<br>在进入正题前，先了解传统的面向对象编程（例如Java）中常会涉及到的概念，大致可以包括：

* 类：定义对象的特征。它是对象的属性和方法的模板定义。
* 对象（或称实例）：类的一个实例。
* 属性：对象的特征，比如颜色、尺寸等。
* 方法：对象的行为，比如行走、说话等。
* 构造函数：对象初始化的瞬间被调用的方法。
* 继承：子类可以继承父类的特征。例如，猫继承了动物的一般特性
* 封装：一种把数据和相关的方法绑定在一起使用的方法。
* 抽象：结合复杂的继承、方法、属性的对象能够模拟现实的模型。
* 多态：不同的类可以定义相同的方法或属性。

<br>在JavaScript的面向对象编程中大体也包括这些。不过在称呼上可能稍有不同，例如，JavaScript中没有原生的“类”的概念， 而只有对象的概念。因此，随着你认识的深入，我们会混用对象、实例、构造函数等概念。
<br>

###对象（类）的创建
<br>在JavaScript中，我们通常可以使用构造函数来创建特定类型的对象。诸如Object和Array这样的原生构造函数，在运行时会自动出现在执行环境中。 此外，我们也可以创建自定义的构造函数。例如：
```javascript
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
}

var person1 = new Person('Weiwei', 27, 'Student');
var person2 = new Person('Lily', 25, 'Doctor');
```
<br>按照惯例，构造函数始终都应该以一个大写字母开头（和Java中定义的类一样），普通函数则小写字母开头。 要创建Person的新实例，必须使用new操作符。以这种方式调用构造函数实际上会经历以下4个步骤：

* 1.创建一个新对象（实例）
* 2.将构造函数的作用域赋给新对象（也就是重设了this的指向，this就指向了这个新对象）
* 3.执行构造函数中的代码（为这个新对象添加属性）
* 4.返回新对象

<br>在上面的例子中，我们创建了Person的两个实例person1和person2。 这两个对象默认都有一个constructor属性，该属性指向它们的构造函数Person，也就是说：

```javascript
console.log(person1.constructor == Person);  //true
console.log(person2.constructor == Person);  //true
```
###自定义对象的类型检测
<br>我们可以使用instanceof操作符进行类型检测。我们创建的所有对象既是Object的实例，同时也是Person的实例。 因为所有的对象都继承自Object。
<br>
```javascript
console.log(person1 instanceof Object);  //true
console.log(person1 instanceof Person);  //true
console.log(person2 instanceof Object);  //true
console.log(person2 instanceof Person);  //true
```
###构造函数的问题
<br>我们不建议在构造函数中直接定义方法，如果这样做的话，每个方法都要在每个实例上重新创建一遍，这将非常损耗性能。 ——不要忘了，ECMAScript中的函数是对象，每定义一个函数，也就实例化了一个对象。
<br>
<br>幸运的是，在ECMAScript中，我们可以借助原型对象来解决这个问题。

###借助原型模式定义对象的方法
<br>我们创建的每个函数都有一个prototype属性，这个属性是一个指针，指向该函数的原型对象， 该对象包含了由特定类型的所有实例共享的属性和方法。也就是说，我们可以利用原型对象来让所有对象实例共享它所包含的属性和方法。

```javascript
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
}

// 通过原型模式来添加所有实例共享的方法
// sayName() 方法将会被Person的所有实例共享，而避免了重复创建
Person.prototype.sayName = function () {
  console.log(this.name);
};

var person1 = new Person('Weiwei', 27, 'Student');
var person2 = new Person('Lily', 25, 'Doctor');

console.log(person1.sayName === person2.sayName); // true

person1.sayName(); // Weiwei
person2.sayName(); // Lily
```
<br>正如上面的代码所示，通过原型模式定义的方法sayName()为所有的实例所共享。也就是， person1和person2访问的是同一个sayName()函数。同样的，公共属性也可以使用原型模式进行定义。例如：

```javascript
function Chinese (name) {
    this.name = name;
}

Chinese.prototype.country = 'China'; // 公共属性，所有实例共享
```

<br>-------------未完--继续-------------
