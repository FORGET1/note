# LESS笔记
## 1.初见LESS
* 什么是LESS
	* Less是一个CSS预编译器，意思指的是它可以扩展CSS语言，添加功能如允许变量，混合，函数和其他的技术，让你的CSS更具维护性，主题性，扩展性。
* LESS官方网站
	* 英文&emsp;[http://www.lesscss.org/](http://www.lesscss.org/)
	* 中文&emsp;[http://www.lesscss.net/](http://www.lesscss.net/)
* 与less的第一次碰面
	* 传统写法
	
		```
		.content ui{
			list-style: none;
		}
		.content li{
			height: 24px;
			line-height: 25px;
			padding-left: 15px;
			background: url("arr.jpg") no-repeat center left;
		}
		.content li a{
			color: #535353;
			text-decoration: none;
			font-family: microsoft yahei;
		}
		```
	* LESS写法
	
		```
		.content ul{
			list-style: none;
			li {
				height: 24px;
				line-height: 25px;
				padding-left: 15px;
				background: url("arr.jpg") no-repeat center left;
				a {
					color: #535353;
					text-decoration: none;
					font-family: microsoft yahei;
				}
			}
		}
		```
	* LESS中注释
		* // 开始的注释不会被编译到css文件中
		* /**/ 包裹的注释会被编译在css文件中

## 2.正确使用less
* 如何使用LESS
	* LESS文件只有在被编译后才能被浏览器识别使用
* LESS编译工具
	* Koala，国人开发的全平台的LESS编译工具。 网址：[http://koala-app.com/](http://koala-app.com/)
	* WinLess，Windows下的LESS编译软件。 网址：[http://winless.org](http://winless.org)
	* CodeKit，MAC平台下的LESS编译软件。网址：[http://incident57.com/codekit/index.html](http://incident57.com/codekit/index.html)
* 客户端调试方式
	* 首先引用less文件&emsp;&emsp;<font color="red">注意：引用时使用link引入，然后将rel属性设置为`rel="stylesheet/less"`</font>
	* 然后引用less.js&emsp;&emsp;<font color="red">注意：与引入普通js方式一致，但是一定要放置在less样式文件之后。</font>

## 3.变量
* 普通的变量
	* 变量的定义方式@
	* 实例
		* less编写
		```
		@blue:#5B83AD;
		#header {
			color: @blue;
		}
		```
		* 编译结果
		```
		#header {
			color: #5B83AD;
		}
		```
	* 注意
		* 由于变量只能定义一次，实际上他们就是“常量”
* 作为选择器和属性名
	* 使用时将变量以@(变量名)的方式使用
	* 实例
		* less编写
		```
		@mySelector:width;
		.@{mySelector} {
			@{mySelector}: 960px;
			height: 500px;
		}
		```
		* 编译结果
		```
		.width {
			width: 960px;
			height: 500px;
		}
		```
* 作为URL
	* 使用时，使用""将变量的值括起来，使用时同样将变量以@(变量名)的方式使用
	* 实例
		* less编写
		```
		@myUrl:"http://www.maiziedu.com/static/images";
		body {
			background: #4CDCA9 url("@{myUrl}/logo.png") no-repeat
		}
		```
		* 编译结果
		```
		body {
			background: #4CDCA9 url("http://www.maiziedu.com/static/images/logo.png") no-repeat
		}
		```
* 延迟加载
	* 什么是延迟加载
		* 变量是延迟加载的，在使用前不一定要先声明
	* 实例
		* less编写
		```
		.lazy-eval {
			width: @val;
		}
		@val:@a;
		@a:9%;
		```
		* 编译结果
		```
		.lazy-eval {
			width: 9%;
		}
		```
* 定义多个相同名称的变量时
	* 在定义一个变量两次时，只会使用最后定义的变量，less会从当前作用域向上搜索，这个行为类似于CSS的定义中始终使用最后定义的属性值。
	* 实例
		* less
		```
		@val:0;
		.class{
			@val:1;
			.brass{
				@val:2;
				three:@val;
				@val:3;
			}
			one:@val;
		}
		```
		* 编译结果
		```
		.class {
		  one: 1;
		}
		.class .brass {
		  three: 3;
		}
		```

## 4.混合
* 普通混合
	* 混合就是一种将一系列属性从一个规则引入("混合")到另一个规则集的方式。
	* 实例
		* less编写
		```less
		.bordered{
			border-top: dotted 1px black;
			border-bottom: solid 2px black;
		}
		#meau a{
			color: #111;
			.bordered;
		}
		.post a{
			color: red;
			.bordered;
		}
		```
		* 编译结果
		```css
		.bordered {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		#meau a {
		  color: #111;
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		.post a {
		  color: red;
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		```
* 不带输出的混合
	* 什么是不带输出的混合
		* 如果你想要创建一个混合集，但是却不想它输出到你的样式中
	* 使用方法
		* 在不想输出的混合集的名字后面加上一个括号，就可以实现！
		* less语句
		```less
		.my-mixin {
			color: black;
		}
		.my-other-mixin() {
			background: white;
		}
		.class {
			.my-mixin;
			.my-other-mixin;
		}
		```
		* 编译结果
		```css
		.my-mixin {
		  color: black;
		}
		.class {
		  color: black;
		  background: white;
		}
		```
* 带选择器的混合
	* less语句
	```less
	.my-hover-mixin() {
		&:hover {
			border: 1px solid red;
		}
	}
	button {
		.my-hover-mixin;
	}
	```
	* 编译结果
	```css
	button:hover {
	  border: 1px solid red;
	}
	```
* 带参数的混合
	* less语句
		* 定义混合
		```less
		.border(@border_color) {
			border: solid 2px @border_color;
		}
		```
		* 使用混合
		```less
		.div {
			.border(#f60);
		}
		```
		<font color="red">使用的时候我们需要传一个参数进去</font>
		* 编译结果
		```css
		.div {
		  border: solid 2px #ff6600;
		}
		```
* 带参数并且有默认值
	* 定义混合
	```less
	.border(@border_color:#f60) {
		border: 2px solid @border_color;
	}
	```
	* 使用混合
	```less
	.div1 {
		.border();
	}
	```
	* 不带参数使用
	```less
	.div1 {
		.border();
	}
	```
	* 不带参数混合编译结果
	```css
	.div1 {
	  border: 2px solid #ff6600;
	}
	```
	* 带参数使用
	```less
	.div {
		.border(red);
	}
	```
	* 带参数的混合编译结果
	```css
	.div {
	  border: 2px solid #ff0000;
	}
	```
* 带多个参数的混合
	* 什么是参数
		* 一个组合可以带多个参数，参数之间可以用分号或者逗号分离
		* 但是推荐使用分号分割，因为逗号符号有两个意思，它可以解释为mixins参数分隔符或者css列表分隔符
	* 官方阐述
		* 两个参数，并且每个参数都是逗号分割的列表：`.name{1,2,3;something,ele}`
		* 三个参数，并且每个参数都包含一个数字：`.name{1,2,3}`
		* 使用伪造的分号创建mixin，调用的时候参数包含一个逗号分割的css列表：`.name{1,2,3;}`
		* 逗号分割默认值：`.name{@param1:red,blue}`
	* 实例分析
		* less语句
			```
			.mixin(@color;@padding:xxx;@margin:2){
				color-3:@color;
				padding-3:@padding;
				margin:@margin @margin @margin @margin;
			}
			.div{
				.mixin(1,2,3;something,ele);
			}
			.div{
				.mixin(1,2,3);
			}
			.div2{
				.mixin(1,2,3;)
			}
			```
		* 编译结果
			```
			.div {
			  color-3: 1, 2, 3;
			  padding-3: something, ele;
			  margin: 2 2 2 2;
			}
			.div {
			  color-3: 1;
			  padding-3: 2;
			  margin: 3 3 3 3;
			}
			.div2 {
			  color-3: 1, 2, 3;
			  padding-3: xxx;
			  margin: 2 2 2 2;
			}
			```
	* 定义多个具有相同名称和参数数量的混合
		* 定义多个具有相同名称和参数数量的mixins是合法的，less会使用它可以应用的属性，如果使用mixin的时候只带一个参数，比如`.mixin{green}`，这个属性会导致所有的mixin都会强制使用这个明确的参数
		* less语句
			```
			.mixin(@color){
				color-1:@color;
			}
			.mixin(@color,@padding:2){
				color-2:@color;
				padding-2:@padding;
			}
			.mixin(@color,@padding,@margin:2){
				color-3:@color;
				padding:@padding;
				margin: @margin @margin @margin @margin;
			}
			.some .selector div{
				.mixin(#008000);
			}
			```
		* 编译结果
			```
			.some .selector div {
			  color-1: #008000;
			  color-2: #008000;
			  padding-2: 2;
			}
			```
* 命名参数
	* 什么是命名参数
		* 引用mixin时可以通过参数名称而不是参数的位置来为mixin提供参数值，任何参数都已通过它的名称来引用。这样就不必按照任意特定的顺序来使用参数
	* less语句
		```
		.mixin (@color:black;@margin:10px;@padding:20px){
			color: @color;
			margin:@margin;
			padding:@padding;
		}
		 .class1{
			.mixin(@margin:20px;@color:#33ACFE);
		} 
		.class2 {
			.mixin(#EFCA44;@padding:40px)
		}
		```
	* 编译结果
		```
		.class1 {
		  color: #33acfe;
		  margin: 20px;
		  padding: 20px;
		}
		.class2 {
		  color: #efca44;
		  margin: 10px;
		  padding: 40px;
		}
		```
* @anguments变量
	* 什么是@anguments变量
		* @anguments代表所有的可变参数
	* 注意
		* A
		@anguments代表所有可变参数，参数的先后顺序就是你的（ ）括号内的参数的先后顺序
		* B
		注意在使用的赋值，值的位置和个数也是一一对应的，只有一个值，把值赋值给第一个，两个值，赋值给第一个和第二个，三个值，分别赋值给第一个......以此类推，但是需要主要的是假如我想给第一个和第三个赋值，你不能写（值1....值3），必须把原来的默认值写上去。
	* 实例1
		* less语句
		```
		.border(@x:solid;@c:red){
			border:21px @arguments;
		}
		```
		* 编译结果
		```
		.div1 {
		  border: 21px solid #ff0000;
		}
		```
	* 实例2
		* less语句
		```
		.border(@w:1px;@x:solid;@c:red;){
			border:@arguments;
		}
		.div1{
			.border();
		}
		```
		* 编译结果
		```
		.div1 {
			border: 1px solid #ff0000;
		}
		```
* 匹配模式
	* 说明
		* 传值的时候定义一个字符，在使用的时候使用哪个字符，就调用哪条规则
	* 实例
		* less语句
		```
		.border(all,@w:5px){
			border-radius: @w;
		}
		.border(t_l,@w:5px){
			border-top-left-radius: @w;
		}
		.border(t_r,@w:5px){
			border-top-right-radius: @w;
		}
		.border(b_l,@w:5px){
			border-bottom-left-radius: @w;
		}
		.border(b_r,@w:5px){
			border-bottom-right-radius: @w;
		}
		```
		* 实例1：less调用
		> 如下，我想让四条边都是圆角，就调用all
		
		```
		.border{
			.border(all,50%);
		}
		```
		* 实例1：编译结果
		```
		.border {
			border-radius:50%;
			width: 50px;
			height:50px;
			background: royablue;
		}
		```
		* 实例2：less调用
		> 如下，我想让右上角的边是圆角，就调用t_r

		```
		.border {
			.border(t_r,50%);
		}
		```
		* 实例2：编译结果
		```
		.border {
			border-top-right-radius: 50%;
			width: 50px;
			height:50px;
			background: royablue;
		}
		```
	* 默认携带参数
	
* 得到混合中变量的返回值
	* 实例
		* less语句
		```
		.average(@x,@y) {
			@average:((@x + @y) / 2);
		}
		div {
			.average(16px, 50px);
			padding: @average;
		}
		```
		* 编译结果
		```
		div {
			padding: 33px;
		}
		```
	* 实例分析
		1. 首先将16px和50px赋值给混合.average进行计算
		2. 将计算结果赋值给变量@average
		3. 然后在div中调用@average的值
		4. 编译后就得到了average的值33px

## 5.嵌套规则
1. 什么是嵌套规则
	* 嵌套规则它模仿了HTML的结构，让我们的css代码更加简洁明了清晰
2. 实例
	* 传统css写法
	```
	#header {
		color: black;
	}
	#header .navigation {
		font-size: 12px;
	}
	#header .logo {
		width: 300px;
	}
	```
	* less的写法
	```
	#header {
		color: black;
		.navigation {
			font-size: 12px;
		}
		.logo {
			width: 300px;
		}
	}
	```
3. 父元素选择器
	* &说明
		* 表示当前选择器的所有父选择器
	* 实例
		* 传统写法
		```
		.bgcolor {
			background: #ffffff;
		}
		.bgcolor a {
			color: #888888;
		}
		.bgcolor a:hover {
			color: #ff6600;
		}
		```
		* less写法
		```
		.bgcolor {
			background: #ffffff;
			a {
				color: #888888;
				&:hover {
					color: #ff6600;
				}
			}
		
		}
		```
		* less编译结果
		```
		.bgcolor {
			background: #ffffff;
		}
		.bgcolor a {
			color: #888888;
		}
		.bgcolor a:hover {
			color: #ff6600;
		}
		```
4. 改变选择器的顺序
	* 说明
		* 将&放到当前选择器之后，就会将当前选择器插入到所有的父选择器之前
	* 实例
		* less语句
		```
		ul {
			li {
				.content & {
					background: red;
				}
			}
		}
		```
		* 编译结果
		```
		.content ul li {
			background: red;
		}
		
		```
5. 组合使用生成所有可能的选择器列表
	* 实例
		* less语句
		```
		p, a, ul, li {
			border-top: 2px dotted #366;
			& & {
				border-top: 0;
			}
		}
		```
		* 编译结果
		```
		p,
		a,
		ul,
		li {
			border-top: 2px dotted #366;
		}
		p p,
		p a,
		p ul,
		p li,
		a p,
		a a,
		a ul,
		a li,
		ul p,
		ul a,
		ul ul,
		ul li,
		li p,
		li a,
		li ul,
		li li {
			border-top: 0;
		}
		```

## 6.运算
1. 运算说明
	* 任何数值，颜色和变量都可以进行运算。
2. 数值型运算
	* less会自动推断数值的单位，所以你不必每一个值都加上单位。
	- **注意:**运算符与值之间必须以空格分开，涉及优先级时以()进行优先级运算。
	* 实例
		* less运算
		```
		.wp {
			width: 450px + 450;
		}
		```
		* 运算结果
		```
		.wp {
			width: 900px;
		}
		```
	* 优先级运算实例
		* less运算
			* 运算思路：wp的宽度=500px-50px之后*2得到900px的值
			* 错误写法
			```
			.wp {
				width: 500px -50 * 2;
			}
			```
			* 错误原因分析
				* 这里先计算\*再计算，那么也就是50*2得到100，500px-100就是400px
			* 错误写法后的编译结果
			```
			.wp {
				width: 400px;
			}
			```
			* 正确写法
			```
			.wp {
				width: (500px -50) * 2;
			}
			```
			* 编译结果
			```
			.wp {
				width: 900px;
			}
			```
3. 颜色值运算
	* less在运算时，先将颜色值转换为rgb模式，然后再转换为16进制的颜色值并返回
	* 实例
		* less运算
		```
		.content {
			background: #000000 + 21;
		}
		```
		* 编译结果
		```
		.content {
			background: #151515;
		}
		```
	* **注意:**既然是转换为rgb模式，我们知道rgb的取值范围是0~255，所以我们计算时不能超过这个区间，超过后默认使用最大值255计算。

## 7.函数
1. 函数介绍
	* Less提供了许多用于转换颜色，处理字符串和进行算术运算的函数，这些函数使用起来非常简单。
2. 实例-A(最常见的rgb()函数)
	* 最常见的的rgb()函数，将rgb模式的值转换为16进制的值
	* 编写less
		```
		.bgcolor {
			background: rgb(0,133,0);
		}
		```
	* 编译结果
		```
		.bgcolor {
			background: #008500;
		}
		```
3. 实例-B(提取蓝色值blue()函数)
	* 编写less
		```
		.bgcolor {
			background: blue(#FFFFFF);
		}
		```
	* 编译结果
		```
		.bgcolor {
			background: 255;
		}
		```

## 8.命名空间
1. 什么是命名空间
	* 将一些需要的混合组合在一起，可以通过嵌套多层id或者class来实现。
2. 实例
	* less编写
		```
		#bgcolor() {
			background: #FFFFFF;
			.a {
				color: #666666;
				&:hover {
					color: #FF6600;
				}
				.b {
					background: #FF0000;
				}
			}
		}
		.bgcolor1 {
			background: #FDFEE0;
			#bgcolor>.a;
		}
		.bgcolor2 {
			#bgcolor>.a>.b;
		}
		```
	* 编译结果
		```
		.bgcolor1 {
		    background: #FDFEE0;
		    color: #666666;
		}
		.bgcolor1:hover {
		    color: #FF6600;
		}
		.bgcolor1 .b {
		    background: #FF0000;
		}
		.bgcolor2 {
		    background: #FF0000;
		}
		```
3. 省略>符号
	* less写法
		```
		#bgcolor() {
			background: #FFFFFF;
			.a {
				color: #666666;
				&:hover {
					color: #FF6600;
				}
				.b {
					background: #FF0000;
				}
			}
		}
		.bgcolor1 {
			background: #FDFEE0;
			#bgcolor .a;
		}
		.bgcolor2 {
			#bgcolor .a .b;
		}
		```
	* 编译结果
		```
		.bgcolor1 {
		    background: #FDFEE0;
		    color: #666666;
		}
		.bgcolor1:hover {
		    color: #FF6600;
		}
		.bgcolor1 .b {
		    background: #FF0000;
		}
		.bgcolor2 {
		    background: #FF0000;
		}
		```

## 9.作用域
1. 什么是作用域
	* Less中的作用域与编程语言中的作用域概念非常相似，首先会在局部查找变量和混合，如果没找到，编译器就会在父作用域中查找，依次类推。
2. 案例
	* less编写
	```
	@color:#ffffff;
	.bgcolor {
		width: 50px;
		a {
			color: @color;
		}
		@color: #ff0000;
	}
	```
	* 编译结果
	```
	.bgcolor {
		width: 50px;
	}
	.bgcolor a {
		color: #ff0000;
	}
	```

## 10.引入
1. 什么是引入
	* 你可以引入一个或多个.less文件，然后这个文件中的所有变量都可以在当前的less项目中使用！
2. 实例
	* （分析）比如之前我们有一个main.less的文件，现在我们想引入这个文件，然后使用它其中的变量。
	 
	* *main.less文件*

	`@wp: 960px;`

	* *style.less*当前项目文件

	```
	@import "main"; //引入main.less文件
	.content {
		width: @wp; //使用main.less中定义的变量@wp
	}
	```

	* 编译结果
	```
	.content{
		width: 960px;
	}
	```
3. 你也可以在项目中引用css文件，但你不能混合到项目中
	* 注意：引用.css文件会被原样输出到编译的文件中！
	* *index.css*包含的内容
	
		```
		.color {
			color: #ff6600;
		}
		```

	* 实例
		* 错误的方式
		```
		@import "main";
		@import "index.css";
		.content {
			width: @wq;
			height: @wq;
			.color; //不能混合css文件中的样式
		}
		```
		* 正确的方式
		```
		@import "main";
		@import "index.css";
		.content {
			width: @wq;
			height: @wq;
		}
		```
	* 编译结果
	```
	@import "index.css";
	.content {
		width: 960px;
		height: 960px;
	}
	```
4. 可带参数
	* once
		* 默认，只包含一次
	* reference
		* 使用less文件但不输出
	* inline
		* 在输出中包含源文件但不加工它
	* less
		* 将文件作为Less文件对象，无论是什么文件扩展名
	* css
		* 将文件作为CSS文件对象，无论是什么文件扩展名
	* multiple
		* multiple，允许引入多次相同文件名的文件

## 11.关键字
1. 什么是!important关键字
	* 在调用的混合集后面追加!important关键字，可以使混合集里面的所有属性都继承!important
2. less语句
	```
	.foo(@bg:#f5f5f5,@color:#900) {
		background: @bg;
		color:@color;
	}
	.unimportant {
		.foo();
	}
	.important {
		.foo() !important;
	}
	```
3. 编译结果
	```
	.unimportant {
	  background: #f5f5f5;
	  color: #900;
	}
	.important {
	  background: #f5f5f5 !important;
	  color: #900 !important;
	}
	```

## 12.条件表达式
1. 带条件的混合
	* 比较运算符
		* \>, >=, =, =<, <, true
	* less语句
		```
		.mixin(@a) when (lightness(@a)>=50%) {
		  background-color: black;
		}
		.mixin(@a) when (lightness(@a)<50%) {
		  background-color: white;
		}
		.mixin(@a) {
		  color: @a;
		}
		.class1 {.mixin(#ddd)}
		.class2 {.mixin(#555)}
		```
	* 编译结果
		```
		.class1 {
		  background-color: black;
		  color: #ddd;
		}
		.class2 {
		  background-color: white;
		  color: #555;
		}
		```
2. 类型检查函数
	* 可以基于值的类型来匹配函数
	* 基本的类型检查函数
		```
		iscolor
		isnumber
		isstring
		iskeyword
		isurl
		```
	* 例子
		```
		.mixin(@a;@b:0) when (isnumber(@b)) {...}
		.mixin(@a;@b:0) when (iscolor(@b)) {...}
		```
3. 单位检查函数
	*  检查一个值除了数字是否是一个特定的单位
	* 基本的单位检查函数
		```
		ispixel
		ispercentage
		isem
		isunit
		```

## 循环
1. 在Less中，混合可以调用它自身，这样，当一个混合递归调用自己，再结合Guard表达式和模式匹配这两个特性，就可以写出循环结构。
2. 实例
	```
	.loop(@counter) when (@counter>0) {
	  .loop((@counter - 1));	//递归调用自身
	  width:(10px * @counter);  //每次调用时产生的样式代码
	}
	div {
	  .loop(5);	//调用循环
	}
	```
3. 编译结果
	```
	div {
	  width: 10px;
	  width: 20px;
	  width: 30px;
	  width: 40px;
	  width: 50px;
	}
	```

## 合并属性
1. “+”逗号分隔所合并的属性值
	* 在需要合并的属性的：的前面加上+就可以完成合并，合并以"，"分隔属性
	* 实例
	 * less
	 	```
		.minix() {
		  box-shadow+: inset 0 0 10px #555;
		}
		.myclass {
		  .minix();
		  box-shadow+: inset 0 0 20px #666;
		}
		```
	 * 编译结果
	 	```
		.myclass {
		  box-shadow: inset 0 0 10px #555, inset 0 0 20px #666;
		}
		```