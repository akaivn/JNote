## Less
### 简介
特点：预编译，提高开发速度
#### 下载 安装 引用
#### 工具
预编译工具：`Koloa`，下载地址为：[Koloa-app.com](http://koala-app.com/)
#### 注释
预编译时的注释可选择可见或不可见
例：
	可见：/* 我是一段可见的注释 */
	不可见：// 我见不得人

### 变量
less中使用 @ 来定义一个变量，我们可以在任何地方引用它们 
例如：选择器、属性名、值

```less
@color: pink;
@margin: margin;
@selector: *; 
```
接下就是如何引用它们
```less
*{
	@{margin}: 0;
	padding: 0;
}
@{selector}{
	width: 100px;
	height: 200px;
	background-color: #eee;
	.box1{
		width: 100px;
		height: 20px;
		background-color: @color;
	}
}
```
一般我们都不太希望去使用定义变量的方式取代选择器和属性名(这样可能会有点别扭)，但有的时候我们需要这样做的时候，less会给我们提供帮助
当我们学习了less的变量后，我们就有必要去了解一下less变量的加载机制
less变量的加载机制使用的是延迟加载，比如下边这段代码的运行结果：

```less
@var: 1;
#box{
	@var: 2;
	.box1{
		height: @var;
		@var: 3;
	}
	@var: 4;
}
#box{
	height: @var;
}
```
经过编译，它会变成如下CSS代码
```css
#box .box1 {
  height: 3;
}
#box {
  height: 1;
}
```
我们可以调换中间的height: @var的位置，如果我们往上挪一层，结果就又会变成这样：
```css
#box .box1 {
  height: 4;
}
#box {
  height: 1;
}
```
所以less的*变量的作用域和延迟加载的机制*，你熟悉了么？
### 嵌套规则
less代码如果嵌套，默认的嵌套规则就是把嵌套的那个元素变成被嵌套元素的后代，选择器中间就会加上空格，但是以下场景，当我们不需要层级嵌套时，我们又该怎么去改变默认的嵌套规则呢？
- :hover

- :active

- :visited

- :link

- :focus

- :after

- :before

- ...

  如果正常嵌套，那么最终编译会变成这样：
```css
#box :hover{
	xxx: xxx
}
```
这显然不是我们希望看到的，所以我们应该手动改变它的默认嵌套规则
例如：

```less
#box{
	width: 100px;
	height: 200px;
	background-color: #eee;
	.box1{
		width: 100px;
		height: 20px;
		background-color: @color;
		&:hover{
			backgroud-color: #000000;
		}
	}
}
```
我们需要在嵌套的元素上加上 `&`
### 混合
混合就是将一系列属性从一个规则集引入到另一个规则集的方式
> 1.普通混合
```less
.commonCenter{
	width: 20px;
	height: 14px;
	color: black;
}
#box{
	.commonCenter;
}
```
这也是默认的混合模式，被抽出来的公用代码样式，会被编译到css文件中，如果我们不想要编译到css中我们可以考虑使用第二种
> 2.不带输出的混合
```less
.commonCenter(){
	width: 20px;
	height: 14px;
	color: black;
}

#box{
	.commonCenter;
}
```
> 3.带参数的混合
传递参数

```less
.commonCenter(@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
#box{
	.commonCenter(22px,23px,pink);
}
```
> 4.带参数并且有默认值的混合
```less
.commonCenter(@w:22px,@h:22px,@c:yellow){
	width: @w;
	height: @h;
	color: @c;
}
#box{
	.commonCenter();
}
```
> 5.带多个参数的混合

上述已经演示，这里不做过多叙述
> 6.命名参数

当有多个参数而我们需要给某个参数传递值，剩下的参数使用默认值时，我们可以使用 `命名参数` 指定给哪些参数传值
```less
.commonCenter(@w:22px,@h:22px,@c:yellow){
	width: @w;
	height: @h;
	color: @c;
}
#box{
	.commonCenter(@w:33px,@c:red);
}
```
> 7.匹配模式

同时列出多种可能的样式代码，根据其中一个参数来判断到底选择渲染哪个代码块，就像java switch 那样
```less
.commonCenter(O,@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
.commonCenter(T,@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
.commonCenter(H,@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
.commonCenter(F,@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
#box{
	.commonCenter(O,10px,10px,red);
}
#box{
	.commonCenter(T,11px,11px,yellow);
}
#box{
	.commonCenter(H,12px,12px,orange);
}
#box{
	.commonCenter(F,13px,13px,purple);
}
```
相同的我们给它抽出来，再定义一个混合
```less
.commonCommon(@_,@w,@h,@c){
	width: @w;
	height: @h;
	color: @c;
}
.commonCenter(O,@w,@h,@c){
	.commonCommon(@w,@h,@c);
}
.commonCenter(T,@w,@h,@c){
	.commonCommon(@w,@h,@c);
}
.commonCenter(H,@w,@h,@c){
	.commonCommon(@w,@h,@c);
}
.commonCenter(F,@w,@h,@c){
	.commonCommon(@w,@h,@c);
}
#box{
	.commonCenter(O,10px,10px,red);
}
#box{
	.commonCenter(T,11px,11px,yellow);
}
#box{
	.commonCenter(H,12px,12px,orange);
}
#box{
	.commonCenter(F,13px,13px,purple);
}
```
`@_`：会保证每次调用其他样式代码块的时候调用这个混合
> 8.@arguments变量

用来代替多形参时的填充
```less
.commonBorder(@style,@w,@color){
	border: @arguments;
	// border: @style @w @color：原来的写法
}
#box{
	.box1{
		.commonBorder(solid,1px,red);
	}
}
```
### 运算
```less
.commonBorder(@style,@w,@color){
	border: @arguments;
}
#box{
	.box1{
		.commonBorder(solid,1px + 10,red); // less中计算，只需要一个方向带单位即可
	}
}
```
### 继承
我们经常会在css代码中看到如下类型的样式代码块
```css
#box,#box1,#box2,#box3{
	color: black;
	cursor: pointer;
}
```
这个就是css为了不浪费网络性能，使用选择器实现的多模块并用公共样式，如果我们在less中单独使用混合，是无法做到上述效果的。此时，继承的概念就出来了
继承的语法格式如下

```less
.common{ // 注意：继承不予许这里有小括号，更不允许支持参数
	width: 10px;
	height: 20px;
}
#box{
	.box1:extend(.common){
	}
	.box2:extend(.common){
	}
}
```

编译后的结果如下：
```css
.common,
#box .box1,
#box .box2 {
  width: 10px;
  height: 20px;
}
```

### 避免编译

有时我们希望less不编译我们的代码，那我们我们的语法格式就可以为：
```less
*{
	margin: 0;
	padding: ~"100 + 100px";
}
```