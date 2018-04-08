# JS系列分享
## 数据类型的划分及转换
在js中数据类型的划分有两种方法，一种官方给出的标准划分，还有一种是根据 typeof 运算符来进行划分，并且两种划分还略有不同

### 标准划分
- 原始类型(基础类型,简单类型)
	- number 数字，number中包含了我们所有的合法数字，无论小数还是整数，其中有几个值要注意一下
		- Number.POSITIVE_INFINITY(也可以直接使用Infinity) 正无穷大
		- Number.NEGATIVE_INFINITY (也可以直接使用 -Infinity) 负无穷大
		- NaN 非数字, 在js中用来表示类型是number，但不是一个合法数字，稍后，我们在详细的说
	- string 字符串, 在 js 中放在一对引号中的 0 到 多个字符，我们就认定它是个字符串
	- boolean 布尔值, true 和 false
	- undefined 未定义
	- null 一个特殊的关键词，用来表示 null，常用情况，在我们获取元素，没有获取到的时候。虽然有些时候，我们也会叫 null 为空对象，但是注意 null 并不能想 对象一样进行属性操作。比如 null.name = "miaov" 这样写的话在浏览器中会报错
	- symbol ES6新增类型 symbol的实例是唯一且匿名的如下列：
		- `console.log(Symbol() == Symbol()); //false` 因为 symbol 的实例，是唯一的所以 两个不同的实例的比较结果只能是个 false
		- `console.log(Symbol()); //false` Symbol的计算结果是匿名的，所以我们在打印的时候，并不能看见它具体的值
- 复合类型(复杂类型)
	- js 中的 复合类型只有一种 就是object (对象)，对象中包含了 我们经常使用的 function 函数 ，Array 数组，{}等

### typeof 划分	

typeof 操作符，会返回一个表示数据类型的字符串。在 typeof 划分中 js 一共被划分成了 7 种数据类型：number、string、boolean、undefined、symbol、object、function。这里要注意 typeof 划分和 标准中定义的区别。typeof 中把 null 归为了 object 类型，但是把 function 单拎出来变成了一种类型
typeof 具体使用方法可以查看以下[示例](typeof.html)

	```
	var num = 1;
	console.log(typeof num);//number
	var num2 = NaN;
	console.log(typeof num2);//number
	var str = "miaov";
	console.log(typeof str);//string
	var is = true;
	console.log(typeof is);//boolean
	var u;
	console.log(typeof u);//undefined
	var fn = function(){
		alert(1);
	}  
	console.log(typeof fn);//function
	var obj = {};
	console.log(typeof obj);//object
	var n = null;
	console.log(typeof n);//object
	var id = Symbol();
	console.log(typeof id);//symbol
	```

### 强制数据类型转换

- Number(val) 方法,将数据转换成数字, 接收的参数，是我们要转换的数据，返回值是转换后的结果。各种数据类型转换结果如下：
	- 字符串类型，使用 Number() 转换时，规则如下：
		- 当整段字符串都复合数字规则时，转换为数字返回
		- 空字符串,直接返回 0
		- 其余情况，直接返回 NaN
	- 布尔值类型，使用 Number() 转换时，true 返回 1，false 返回 0
	- null，使用 Number() 转换时 返回 0
	- undefined，使用 Number() 转换时 返回 NaN
	- symbol, 使用 Number() 转换时 直接报错
	- 对象类型，使用 Number() 转换时，会先调用对象的 valueOf 方法，然后调用的对象的toString()方法，然后再次依照前面字符串的转换规则进行转换
- parseInt(string, radix) 方法，将数据转换为整数返回
	- 第一个参数接受的是个字符串也就是我们要转换的数据
	- 第二个参数 基数，也可以理解为 标注出我们要转换的这个字符串是几进制的数字	 
	- 转换规则如下，也可以查看：
		- 字符串类型的数据转换规则，会从整段字符串的第一位开始一位一位向后匹配，知道匹配到某一位不符合合法整数规则，就把这位之前提取到的数字返回，如果第0位起就不符合整数规则则直接返回NaN
		- 其他非字符串类型的数据，会先转换成字符串之后，然后在参考上述规则转换,具体[示例](parseint.html)如下:

		```
			var a = 12.2;
			console.log(parseInt(a));// 12
			var b = "12abc";
			console.log(parseInt(b));// 12	
			var c = "a12bc";
			console.log(parseInt(c));// NaN
			var d = "";
			console.log(parseInt(d));// NaN
		```

	- 关于 基数 的注意事项, 在使用 parseInt 的时候，最好设置一下基数，虽然大部分情况下 parseInt 会 按照十进制识别我们传入的数据，但是一些特殊的情况下，还是会出问题，[示例](radix.html)如下:

		```
			var a = '0xf';
			console.log(parseInt(a));// 15 这里因 0x是16进制的前缀所以这里识别成了 16 进制的数据，转换结果为 15
			var b = "070";
			console.log(parseInt(b));// 新版本为 70，老版本部分浏览器 会识别成 56 因为 0是 八进制的标示，这里按照了八进制进行了处理	
		```
- parseFloat(string) 转换成浮点数(也就是小数)，转换规则和parseInt类似，唯一的区别就是parseFloat可以多转换一个小数点	

- String(val) 转换成字符串
	- 数字类型，直接转换数据类型原样返回	
	- undefined ，直接返回字符串 undefined
	- null，直接返回字符串 null
	- 布尔值，直接返回字符串 true 或者 false
	- 函数，直接把整个函数变成字符串 返回
	- symbol，返回一个字符串 "Symbol()"
	- 对象，直接调用对象的 toString 方法

- Boolean(val) 转换成布尔值
	- 数字类型：非零的合法数字转换为 true, 零代表 false, NaN 代表 false
	- 字符串类型：空字符串转换为false，非空字符串转换为true
	- null：转换为false
	- 对象：转换为true
	- symbol：转换为true
	- undefined：转换为false 		

### NaN

前边我们用到了很多 NaN，那 NaN 到底是什么呢？NaN 是 js 中一个很特殊的值, NaN 是 not a number 的简写，从字面上来看就是这不是一个数字，但是 NaN 的数据类型，是属于数字类型的，这就有些好玩了，简单的来说，NaN 是指 数据类型是数字，但是本身内容又不属于合法数字的数据

- 在使用 NaN 时，要注意 NaN 不等于任何值，包括它自己，也就是 NaN 不等于 NaN。
- 当我们要检测一条数据是否是 NaN 时，可以使用 isNaN(val) 来进行检测, 在 isNaN 方法中，传入的数据 能被转换成 合法数字时，就会返回 false，当传入的数据不能被转换成 合法数字( 也就是NaN) 时，isNaN 就会返回 true 

强制类型转相关的内容，我们就说到这里了，下一次我们会接着讲，各种运算符带来的隐式类型转换
