# js-
<br>1、join是数组转化为字符串   array.join(“  ”)中间的是每个数组的项链接的时候用什么链接，而字符串转化为数组的方法是string.split()方法   split方法只有一个参数，表示由字符串哪个符号开始分割为数组，若是空字符串，则返回的数组是字符串每个字符，适用于字符串的每个字符解析！！！！
<br>2、数组的toString()和valueOf方法输出的都是每项由逗号隔开的，函数的toStrong方法是输出函数的源代码.
<br>3、Concat()参数值将被添加原来的末尾，返回一个新的。Slice()方法是切割的问题，切割从哪到哪或者从哪到结束
<br>4、数组的push()方法是在末尾添加一个项，而pop是删除最后一个项  但是var a=array.pop()返回的是删除掉的，shift()是删除数组的第一项，unshift是把一个项放在第一个,然后把余下的向后移位，所以通过shift和push可以有队列的感觉
<br>5、Reverse方法是颠倒数组项的顺序，sort是将数组升序，是按字母顺序，而对于数字，就有问题了
<br>6、Splice()是把数据项插入数组中部，或者删除中部，删除的参数两个：要删除的第一个项起，和要删除多少个，替换不删除：三个参数：起始位置，替换的个数，替换的项。后面参数可以使替换插入的更多的项
<br>7、替换并删除：三个参数：起始位置，删除的个数，插入的项。后面参数可以使替换插入的更多的项。
<br>8、Js中不存在独立的函数，每个函数都是一个对象的方法。Pars
<br>9、eInt   parseFloat 这些全局函数都是Global内置对象的方法，encodeURI()处理完整的url，基本只处理空格，但不处理特殊的字符，和encodeURIComponent()处理一个片断的url，并且处理所有的非标准字符 冒号反斜杠什么的   都用于浏览器的编码。而decodeURI是解码
<br>10、Eval方法在js中很强大，是一个解释程序，eval调用内部引用的变量可在参数以外定义。例如：var a=”aaaaa”  eval(“alert(msg)”);
<br>11、Global对象还有一些属性。例如undefined   NaN等
<br>12、所有非本地对象都是宿主对象，所有BOM和DOM对象都是宿主对象
<br>13、Js只存在一种作用域，公用作用域，所有属性和方法默认都是公用的，而在属性名前后加下划线，开发者表示是私有作用域，只是让开发者自己知道是私有的，但是不会改变他是公用的作用域。
<br>14、构造函数是函数，函数是对象，对象有属性和方法。所以也就可以在一个函数再定义一个函数
<br>15、定义类和对象 的方式1:工厂方式，也就是定义一个方法，会返回一个对象，也就是新生成那个对象。然后要新建对象，直接调用方法就像，这样就有公共的属性和方法，可以自己创建新的时候修改

----------继续记录------------
