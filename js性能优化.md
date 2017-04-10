## js性能问题
随着网络的发展，网速和机器速度的提高，越来越多的网站用到了丰富客户端技术。而现在Ajax则是最为流行的一种方式。JavaScript是一种解释型语言，所以能无法达到和C/Java之类的水平，限制了它能在客户端所做的事情，为了能改进他的性能，我想基于我以前给JavaScript做过的很多测试来谈谈自己的经验，希望能帮助大家改进自己的JavaScript脚本性能。
<br>
<br>语言层次方面
### 循环
循环是很常用的一个控制结构，大部分东西要依靠它来完成，在JavaScript中，我们可以使用for(;;),while(),for(in)三种循环，事实上，这三种循环中for(in)的效率极差，因为他需要查询散列键，只要可以就应该尽量少用。for(;;)和while循环的性能应该说基本（平时使用时）等价。
而事实上，如何使用这两个循环，则有很大讲究。我在测试中有些很有意思的情况，见附录。最后得出的结论是：
如果是循环变量递增或递减，不要单独对循环变量赋值，应该在它最后一次读取的时候使用嵌套的++或—操作符。
如果要与数组的长度作比较，应该事先把数组的length属性放入一个局部变量中，减少查询次数。
###局部变量和全局变量
局部变量的速度要比全局变量的访问速度更快，因为全局变量其实是全局对象的成员，而局部变量是放在函数的栈当中的。
###不使用Eval
使用eval相当于在运行时再次调用解释引擎对内容进行运行，需要消耗大量时间。这时候使用JavaScript所支持的闭包可以实现函数模版（关于闭包的内容请参考函数式编程的有关内容）
### 减少对象查找
因为JavaScript的解释性，所以a.b.c.d.e，需要进行至少4次查询操作，先检查a再检查a中的b，再检查b中的c，如此往下。所以如果这样的表达式重复出现，只要可能，应该尽量少出现这样的表达式，可以利用局部变量，把它放入一个临时的地方进行查询。
这一点可以和循环结合起来，因为我们常常要根据字符串、数组的长度进行循环，而通常这个长度是不变的，比如每次查询a.length，就要额外进行一个操作，而预先把var len=a.length，则就少了一次查询。
### 字符串连接
如果是追加字符串，最好使用s+=anotherStr操作，而不是要使用s=s+anotherStr。
如果要连接多个字符串，应该少使用+=，如
s+=a;s+=b;s+=c;应该写成
s+=a + b + c；而如果是收集字符串，比如多次对同一个字符串进行+=操作的话，最好使用一个缓存。怎么用呢？使用JavaScript数组来收集，最后使用join方法连接起来，如下
```javascript
 var buf = new Array();
 for(var i = 0; i < 100; i++){ 
     buf.push(i.toString());
 }
 var all = buf.join("");//类型转换

```
##### 类型转换是大家常犯的错误，因为JavaScript是动态类型语言，你不能指定变量的类型。

* 1.把数字转换成字符串，应用"" + 1，虽然看起来比较丑一点，但事实上这个效率是最高的，性能上来说：
    * ("" +) > String() > .toString() > new String()这条其实和下面的“直接量”有点类似，尽量使用编译时就能使用的内部操作要比运行时使用的用户操作要快。
    * String()属于内部函数，所以速度很快，而.toString()要查询原型中的函数，所以速度逊色一些，new String()用于返回一个精确的副本。
* 2.浮点数转换成整型，这个更容易出错，很多人喜欢使用parseInt()，其实parseInt()是用于将字符串转换成数字，而不是浮点数和整型之间的转换，我们应该使用Math.floor()或者Math.round()。
另外，和第二节的对象查找中的问题不一样，Math是内部对象，所以Math.floor()其实并没有多少查询方法和调用的时间，速度是最快的。
  
* 3.对于自定义的对象，如果定义了toString()方法来进行类型转换的话，推荐显式调用toString()，因为内部的操作在尝试所有可能性之后，会尝试对象的toString()方法尝试能否转化为String，所以直接调用这个方法效率会更高

### 使用直接量
其实这个影响倒比较小，可以忽略。什么叫使用直接量，比如，JavaScript支持使用[param,param,param,...]来直接表达一个数组，以往我们都使用new Array(param,param,...)，使用前者是引擎直接解释的，后者要调用一个Array内部构造器，所以要略微快一点点。
同样，var foo = {}的方式也比var foo = new Object();快，var reg = /../;要比var reg=new RegExp()快。
### 字符串遍历操作
对字符串进行循环操作，譬如替换、查找，应使用正则表达式，因为本身JavaScript的循环速度就比较慢，而正则表达式的操作是用C写成的语言的API，性能很好。
### 高级对象
自定义高级对象和Date、RegExp对象在构造时都会消耗大量时间。如果可以复用，应采用缓存的方式。
### DOM相关
### 插入HTML
很多人喜欢在JavaScript中使用document.write来给页面生成内容。事实上这样的效率较低，如果需要直接插入HTML，可以找一个容器元素，比如指定一个div或者span，并设置他们的innerHTML来将自己的HTML代码插入到页面中。
### 对象查询
使用[“”]查询要比.items()更快，这和前面的减少对象查找的思路是一样的，调用.items()增加了一次查询和函数的调用。
### 创建DOM节点
通常我们可能会使用字符串直接写HTML来创建节点，其实这样做
### 无法保证代码的有效性
### 字符串操作效率低
所以应该是用document.createElement()方法，而如果文档中存在现成的样板节点，应该是用cloneNode()方法，因为使用createElement()方法之后，你需要设置多次元素的属性，使用cloneNode()则可以减少属性的设置次数——同样如果需要创建很多元素，应该先准备一个样板节点。
### 定时器
如果针对的是不断运行的代码，不应该使用setTimeout，而应该是用setInterval。setTimeout每次要重新设置一个定时器。
### 其他
### 脚本引擎
据我测试Microsoft的JScript的效率较Mozilla的Spidermonkey要差很多，无论是执行速度还是内存管理上，因为JScript现在基本也不更新了。但SpiderMonkey不能使用ActiveXObject
### 文件优化
文件优化也是一个很有效的手段，删除所有的空格和注释，把代码放入一行内，可以加快下载的速度，注意，是下载的速度而不是解析的速度，如果是本地，注释和空格并不会影响解释和执行速度。
### 总结
总结了我在JavaScript编程中所找到的提高JavaScript运行性能的一些方法，其实这些经验都基于几条原则：

* 直接拿手头现成的东西比较快，如局部变量比全局变量快，直接量比运行时构造对象快等等。
* 尽可能少地减少执行次数，比如先缓存需要多次查询的。
* 尽可能使用语言内置的功能，比如串链接。
* 尽可能使用系统提供的API，因为这些API是编译好的二进制代码，执行效率很高
* 同时，一些基本的算法上的优化，同样可以用在JavaScript中，比如运算结构的调整，这里就不再赘述了。但是由于JavaScript是解释型的，一般不会在运行时对字节码进行优化，所以这些优化仍然是很重要的。
<br>
<br>当然，其实这里的一些技巧同样使用在其他的一些解释型语言中，大家也可以进行参考。
<br>
<br>由于是以前做过的测试，测试代码已经不全，我补充了一部分如下：

```javascript
 var print; 
if(typeof document != "undefined" ){ 
  print = function(){ 
document.write(arguments[0]); 
} 
}else if(typeof WScript != "undefined" ){ 
  print = function(){ 
    WScript.Echo(arguments[0],arguments[1],arguments[2]); 
  } 
} 
function empty(){ 
} 
function benchmark(f){ 
  var i = 0; 
  var start = (new Date()).getTime(); 
  while(i < pressure){ 
    f(i++); 
  } 
  var end = (new Date()).getTime(); 
  WScript.Echo(end-start); 
} 
/* 
i=0 
start = (new Date()).getTime(); 
while(i < 60000){ 
  c = [i,i,i,i,i,i,i,i,i,i]; 
  i++; 
} 
end = (new Date()).getTime(); 
WScript.Echo(end-start); 
i=0 
start = (new Date()).getTime(); 
while(i < 60000){ 
  c = new Array(i,i,i,i,i,i,i,i,i,i); 
  i++; 
} 
var end = (new Date()).getTime(); 
WScript.Echo(end-start); 
*/
function internCast(i){ 
  return "" + i; 
} 
function StringCast(i){ 
  return String(i) 
} 
function newStringCast(i){ 
  return new String(i) 
} 
function toStringCast(i){ 
  return i.toString(); 
} 
function ParseInt(){ 
  return parseInt(j); 
} 
function MathFloor(){ 
  return Math.floor(j); 
} 
function Floor(){ 
  return floor(j); 
} 
var pressure = 50000; 
var a = ""; 
var floor = Math.floor; 
j = 123.123; 
print("-------------\nString Conversion Test"); 
print("The empty:", benchmark(empty)); 
print("intern:", benchmark(internCast)); 
print("String:"); 
benchmark(StringCast); 
print("new String:"); 
benchmark(newStringCast); 
print("toString:"); 
benchmark(toStringCast); 
print("-------------\nFloat to Int Conversion Test"); 
print("parseInt"); 
benchmark(ParseInt); 
print("Math.floor"); 
benchmark(MathFloor); 
print("floor") 
benchmark(Floor); 
function newObject(){ 
  return new Object(); 
} 
function internObject(){ 
  return {}; 
} 
print("------------\nliteral Test"); 
print("runtime new object", benchmark(newObject)); 
print("literal object", benchmark(internObject));
```

代码1：
```javascript
for(var i=0;i<100;i++){ 
    arr[i]=0; 
  }
  ```
代码2：
```javascript
var i = 0; 
 while(i < 100){ 
   arr[i++]=0; 
 }
 ```
 代码3： 
```javascript
var i = 0; 
 while(i < 100){ 
   arr[i]=0; 
   i++; 
 }
 ```
 在firefox下测试这两段代码，结果是代码2优于代码1和3，而代码1一般优于代码3，有时会被代码3超过；而在IE 6.0下，测试压力较大的时候（如测试10000次以上）代码2和3则有时候优于代码1，有时候就会远远落后代码1，而在测试压力较小（如5000次），则代码2>代码3>代码1。
代码4：
```javascript
var i = 0; 
  var a; 
  while(i < 100){ 
    a = 0; 
    i++; 
  }
  ```
代码5：
```javascript
var a; 
 for(var i=0;i<100;i++){ 
   a = 0; 
 }
 ```
上面两段代码在Firefox和IE下测试结果都是性能接近的。
代码6：
```javascript
var a; 
var i=0; 
while(i<100){ 
  a=i; 
  i++; 
}
```
代码7：
```javascript
var a; 
 var i=0; 
 while(i<100){ 
   a=i++; 
 }
 ```
代码8：
```javascript
var a; 
 for(var i=0;i<100;i++){ 
   a = i; 
 } 
 ```
代码9：
```javascript
var a; 
 for(var i=0;i<100;){ 
   a = i++; 
 }
 ```
这四段代码在Firefox下6和8的性能接近，7和9的性能接近，而6, 8 < 7, 9；
最后我们来看一下空循环
代码10： 
```javascript
for(var i=0;i<100;i++){  }
```
代码11：
```javascript
var i; 
while(i<100){    i++;  }
```
最后的测试出现了神奇的结果，Firefox下代码10所花的时间与代码11所花的大约是24:1。所以它不具备参考价值，于是我没有放在一开始给大家看。
