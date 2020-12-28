```
{{$store.getters.returnVulueForParam(20)}}
```

# Vue

## 1、Vue简介

百度有

### 1.2、Vue安装

```txt
1、CDN引入
2、下载引入
3、npm安装 以后使用的方式
```

### 1.3、Vue实例说明

例：

```javascript
const app = new Vue({ // es6 JavaScript多出了另外两个定义变量和方法的修饰符 1、let 代表变量 2、const 代表常量
		el:'#app', // Vue 指定要挂载的dom元素，只有在指定挂载的dom元素中才有vue加持的作用，之外是没有的， 范围是此dom元素之内
		data:{ // Vue的数据都放在这里，使用 ，分割，一般都是服务器响应回来数据后将数据放到这里再做后续显示
			firstname: 'Tc',
			lastname: 'l'
		},
		computed:{ // 计算属性，用于计算一些复杂的操作，有缓存，且页面初始化时只会被执行一次，另外再刷新从缓存中取，调用时直接写属性名即可
			fullName:function(){
				return this.firstname + ' ' + this.lastname;
			}
		},
		methods: { // 这里面定义这方法，事件监听的方法也在这里，调用时要加()
			fullname:function(){
				return this.firstname + ' ' + this.lastname;
			}
		}
	})
```



## 2、Vue常用指令

```javascript
// 绑定
v-bind   语法糖为： :
例：v-bind:src=""  :src=""
// 双向绑定
v-model
// 事件监听
v-on:
// 相当于innerText或者JQuery中的text()函数
v-text
// 相当于innerHtml或者JQuery中的html()函数
v-html
// 原封不动的显示
v-pre
// 斗篷遮挡
v-cloak
// 样式写法为[v-cloak]{display: none;}  html 代码应用为： <span v-cloak>{{sex}}</span>

// 值不会发生改变
v-once
// 循环
v-for
v-for 遍历对象时，如果采用常规写法，拿出来的是value，拿出key时应使用 v-for="(v,k) in items" ,第一个为值，索引值写法为
v-for="(v,k,index) in items"
// 判断
v-if v-if-else v-else
v-show
```

### 2.1、v-on

```javascript
// 事件监听
v-on:    语法糖为： @   可以支持传参，或者默认参数为此次监听的事件对象 event
例：v-on:click or v-on:mouseover   @click @mouseover
// v-on 的修饰符
.stop //阻止事件冒泡 即事件联动
.prevent //阻止表单自动提交
键盘事件.enter // 表示只监听回车键
.native // 表示监听原生时间
.once // 只触发一次回调
```

### 2.2、v-if

```html
<h4 v-if="flag">flag为ture</h4>
<h4 v-else-if="flag">flag2</h4>
<h4 v-else>flag为false显示我</h4>

<!--逻辑太复杂建议使用计算属性，在conputed内部使用if else if else-->

考虑到vue的虚拟dom渲染的复用机制，我们可以通过加 key="" 来解决，当vue进行复用时会首先对比key，如果key一致，就会复用
```

### 2.3、v-bind

```javascript
// v-bind 支持绑定一个对象(obj)，或者类样式(class)，或者支持绑定一个计算属性，或者支持一个数组
// 当绑定的值是一个class时,当boolean值为true时就加上此类名
<span :class="{类名: boolean}"></span>
// vuejs使普通的class和v-bind绑定的class能够共存
<span :class="{类名: boolean}" class="xxx"></span>

// 当绑定的是一个对象时
<span :class="obj"></span>
// main.js代码为:
data: {
  obj: {
    active: true,
    'text-danger': false
  }
}

// 当绑定的是一个计算属性时
<span :class="arithmetic"></span>
// main.js代码为:
computed: {
  arithmetic(){
    return {
        active: true,
    	'text-danger': false
    }
  }
}

// 当绑定的是一个数组时
<span :class="[activeClass, errorClass]"></span>
// main.js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

### 2.4、v-model

双向绑定，即原本由vuejs渲染到页面的数据改变时，页面数据发生变化，页面数据发生变化时，vuejs数据也发生变化

```javascript
<div id="app">
    <input type="text" v-model="message"/>
        <span>{{message}}</span>
</div>
```

基本使用如上

==原理：内部使用 v-bind: 和  v-on:input这一个指令和事件完成的配合==

```javascript
<input type="text" :value="message" @input="message = $event.target.value"/>
// 注：input是表单框的内置事件，监听用户输入时的状态，$event拿到原生浏览器的监听对象，再通过这里拿到值赋值给message
```

#### 2.4.1、v-model和单选框

html

```html
<div id="app">
    <input type="radio" <!--name="sex"--> value="man" v-model="sex"> 男 <!--如果此处使用v-model绑定的是同一个vuejs属性，那么name属性就可以不写-->
    <input type="radio" <!--name="sex"--> value="woman" v-model="sex"> 女
    <br/>
    你选择的性别是： {{sex}}
</div>
```

Vuejs

```javascript
const app = new Vue({
    el:"#app",
    data:{
        message: 'xxx',
        sex: 'woman'
    },
})
```

#### 2.4.2、v-model和复选框

##### 2.4.2.1、单个复选框用法

html

```html
<div id="app">
    <input type="checkbox" value="basketbool" v-model="isAgree"/> 同意协议<a href="www.baidu.com">协议说明</a>
    <button :disabled="!isAgree">下一步</button>
</div>
```

Vuejs

```javascript
const app = new Vue({
    el:"#app",
    data:{
        message: 'xxx',
        isAgree: false
    }
})
```

##### 2.4.2.2、多个复选框的用法

html

```html
<div id="app">
    <input type="checkbox" value="Sing" v-model="hobbies"> Sing
    <input type="checkbox" value="Dance" v-model="hobbies"> Dance
    <input type="checkbox" value="Rap" v-model="hobbies"> Rap
    <input type="checkbox" value="Lanqiu" v-model="hobbies"> Lanqiu
    <span v-for="hobby in hobbies">{{hobby}}</span><br/>
</div>
```

Vuejs

```javascript
const app = new Vue({
    el:"#app",
    data:{
        message: 'xxx',
        hobbies: ['Sing',"Rap"]
    }
})
```

多个复选框是数组形式的数据，单个的是boolean类型的格式

#### 2.4.3、v-model和下拉框

html

```html
<select name="fruit" v-model="fruit">
    <option value="葡萄">葡萄</option>
    <option value="西瓜">西瓜</option>
    <option value="桃子">桃子</option>
    <option value="菠萝">菠萝</option>
    <option value="香橙">香橙</option>
</select>
```

Vuejs

```javascript
const app = new Vue({
    el:"#app",
    data:{
        message: 'xxx',
        fruit: '香橙'
    }
})
```

#### 2.4.4、v-model的修饰符

```html
.lazy: 失去焦点时才会进行更新
.number: 只允许输入数字类型
<input type = "number" v-model.number=""/>
.trim: 去除两边空格

```



### 2.5、值绑定

html

```html
<div id="app">
    <select name="fruit" v-model="fruit">
        <option :value="f" v-for="f in fruits">{{f}}</option>
    </select>

    <span>您选择的是： <span v-cloak>{{fruit}}</span></span>
</div>
```

Vuejs

```javascript
const app = new Vue({
    el:"#app",
    data:{
        message: 'xxx',
        fruits:['香橙','西瓜','桃子','菠萝','葡萄'],
        fruit: '香橙'
    }
})
```

==值绑定指的就是动态绑定数据，也就是再说  v-bind:== ,意在数据不是在页面写死的，而是服务器拿出来渲染上去的

## 3、计算属性(computed)

### 3.1、计算属性的简单用法

```html
<div id="app">
    <h3>{{firstname + ' ' + lastname}}</h3>
    <h3>{{firstname}} {{lastname}}</h3>
    <h3>{{fullname()}}</h3> 
    <h3>{{fullName}}</h3><!-- 使用计算属性时就不用带()-->
</div>
```

```javascript
<script>
	const app = new Vue({
		el:'#app',
		data:{
			firstname: 'Tc',
			lastname: 'l'
		},
		computed:{
			fullName:function(){
				return this.firstname + ' ' + this.lastname;
			}
		},
		methods: {
			fullname:function(){
				return this.firstname + ' ' + this.lastname;
			}
		}
	})
</script>
```

### 3.2、计算属性的复杂用法

```html
<div id="app">
    <h3>{{totalPrice}}</h3>
</div>
```

```javascript
const app = new Vue({
    el:'#app',
    data:{
        books: [
            {'id': 100,'price': 1000},
            {'id': 101,'price': 90},
            {'id': 102,'price': 200}
        ]
    },
    computed:{
        totalPrice:function(){
            // es5常规用法
            var totalPrice = 0;
            for (var i = 0; i < this.books.length; i++) { // 此处的this一定要写，表明这是指向的 app 这个对象里面的数据
                totalPrice += this.books[i].price; // 此处也要写明this，this很重要！！！
            }
            return totalPrice;
        }
        // 通过遍历下标来拿，语法更简洁
        totalPrice:function(){
				// es6写法
				var totalPrice = 0;
				for (let index in this.books) {
					totalPrice += this.books[index].price;
				}
				return totalPrice;
			}
       	// 直接拿到每一本书
        totalPrice:function(){
				// es6写法2
				var totalPrice = 0;
				for (let book of this.books) {
					totalPrice += book.price;
				}
				return totalPrice;
			}
    }
})
```

### 3.3、计算属性的getter和setter

```javascript
computed:{
    // fullname:function(){
    // 	return this.firstname + this.lastname;
    // }

    // 上面那种写法的真实写法其实是这样的
    //fullname:{ // 这是一个计算 属性
    // 内置一个set方法和一个get方法
    // 默认一般不会重写set方法，调用的都是get方法，所以它是一个只读属性
    // set:function(){

    // },
    // get:function(){
    // 此时再次运行我们就会发现它的值并没有发生变化
    // return this.firstname + this.lastname;
    // },
    //},

    //由此我们觉得get方法这样写太麻烦，所以我们选择简写，即为最上面的写法
    fullname:function(){
        // 这也是为什么，它明明是一个方法，我们可以不加()就调用
        return this.firstname + this.lastname;
    }
}
```

### 3.4、计算属性和methods的区别

1、计算属性是有缓存的，只会在界面加载时调用一次，之后的每次调用都会去缓存中拿值然后渲染到页面上，如果你修改了计算属性的值，那么值就会发生变化，也会重新执行缓存

2、methods中的方法，你调用几次就会执行几次，效率像对于计算属性来说较低

## 4、ES6

### 1、let和var的区别

1、let拥有块级作用域，var没有块级作用域

没有块级作用域就代表不管是变量还是函数不管定义在哪里都是全局可用的，在安全性上显然不太好

2、let不能在定义之前访问该变量，但是var是可以得。也就是说，let必须是先定义，再使用，而var先使用后声明也行，只不过直接使用但是没有却没有定义的时候，其值为undefined

3、 let不能被重新定义，但是var是可以的

总之呢，let从规范化的角度来说，要比var要进步了很大一步。所以一般情况下的话，推荐用let，const这些

### 2、const的使用

将一个方法或者一个变量修饰为一个常量，推荐优先使用

1、使用const修饰一个变量时此必须要有初始值

2、常量定义的规则是被const修饰的变量不能变，但变量下的属性可以变

例：

```javascript
const obj = {
    name： 'abc',
    age: 15
}
obj = {}; // 错误的更改，编译时报错
obj.name = "loby"; // 这个就可以修改 
```

### 3、对象字面量增强写法

#### 1、变量的增强写法

例：

```java
const name = 'aaa';
const age = 10;

// es5的常规写法
const obj = {
    name: name,
    age: age
}

// es6
const obj = {
    name,
    age // 解析时会将变量的属性解析为key，将变量值解析为value
}
```

#### 2、方法的增强写法

例：

```javascript
// es5
var obj = {
    eat:function(){
    
	},
    hi:function(){
        
    }
}

// es6
const obj = {
    eat(){
        
    },
    hi(){  // vue 中的方法都可以使用这种方式来声明和定义
        
    }
}
```

## 5、JS高阶函数

### 5.1、filter

```javascript
const arr = [10,20,30,40,555,444,333];
// 1、将所有的数过滤，只要100以下的
let arr2 = arr.filter(function(n){
    return n < 100;
    // filter函数每次返回必须是一个boolean类型，返回为一个新的数组
    // 如果为true，则将这个元素添加到一个新的数组
    // 如果为false，则将这个元素过滤掉
})
```

### 5.2、map

```javascript
// 2、将所有的数 * 2
let arr3 = arr2.map(function(n){
    return n * 2;
    // 经计算后，map函数会返回一个新的数组
    // 将每次计算后的值映射到新的数组中
})
```

### 5.3、reduce

```javascript
// 3、将所有的数加到一起，计算总和
let sum = arr3.reduce(function(prevValue,n){
    return prevValue + n;

    // reduce函数将所有的数以某种方式综合到一起，返回一个新值
    // 第一个参数为回调函数，第二个参数为返回新值的初始值
    // 函数的第一个参数为遍历的前一个值，第二个是传入的值
    // 每次相加就能综合算出总数
},0)
```

### 5.4、精简解决方式

==函数式编程/链式调用==

```javascript
const arr = [10,20,30,40,555,444,333];

let arr2 = arr.filter(function(n){
    return n < 100;
}).map(function(n){
    return n * 2;
}).reduce(function(prevValue,n){
    return prevValue + n;
},0)
```

以后会有更加简单的解决方式：==箭头函数  =>==

## 6、组件化

### 6.1、创建组件的步骤

```txt
1、创建一个组件构造器
	Vue.extend()
2、注册组件
	Vue.component()
3、使用组件
```

示例:

```javascript
// 1、创建组建构造器
const cpn = Vue.extend({
    template:
    `<div>
<h2>标题</h2>
<p>内容</p>
</div>` // `` 为es6语法，这里面的字符串会随着你的换行而换行
})

// 2、注册组件
Vue.component("cpn-m",cpn); // 第一个参数为：组件名，第二个参数为：组件构造器对象
```

```html
<div id="app">
    <!-- 3、使用组件 -->
    <cpn-m></cpn-m>
    <cpn-m></cpn-m>
    <cpn-m></cpn-m>
    <cpn-m></cpn-m>
    <cpn-m></cpn-m>
</div>
```

### 6.2、局部组件的定义

```javascript
components:{
    cpn1: cpn // 第一个参数为：组件名，第二个参数为：组件构造器对象
}
```

### 6.3、组件语法糖

```javascript
//1、注册全局组件语法糖  内部还是会调用Vue.extend进行组件构造，然后传入
Vue.component("cpn1",{
    template:
    `
<div>
<h3>标题1</h3>
<p>内容1</p>
</div>
`
})

// 2、注册局部组件语法糖
const app = new Vue({
		el:"#app",
		data:{
		},
		components:{
			"cpn1":{
				template:
					`
						<div>
							<h3>标题1</h3>
							<p>内容1</p>
						</div>
					`
			}
		}	
	})
```



### 6.4、父子组件

```html
<div id="app">
    <!-- 3、使用组件 -->
    <cpn22></cpn22>
</div>
```

```javascript
// 1、组件构造器
	const cpn = Vue.extend({
		template:
			`
			<div>
				<h3>标题1</h3>
				<p>内容1</p>
			</div>
			`
	})
	
	// 2、组件构造2
	const cpn2 = Vue.extend({
		template:
			`
				<div>
					<h3>标题2</h3>
					<p>内容2</p>
					<cpn1></cpn1>
				</div>
			`,
		components:{
			cpn1: cpn
		}
	})
	
	const app = new Vue({
		el:"#app",
		data:{
		},
		components:{
			cpn22: cpn2
		}
	})
```

### 6.5、组件模板抽离

**1、**

```html
<script type = "text/x-template" id = "temp">
				<cpn1></cpn1>
</script>
```

**2、**

```html
<template id="temp2">
	跟上边一样
</template>
```

Vuejs

```javascript
// 1、全局注册时使用
Vue.component("cpn2",{
    template:"#temp2"
})

//2、局部注册时使用
const app = new Vue({
    el:"#app",
    data:{
    },
    components:{
        "cpn1": {
            template: "#temp"
        }
    }
})
```

### 6.6、组件中的数据存放

==组件想要调用vue实例的数据是不允许的，它有自己的数据存放的地方，不可能全部挂载到vue实例下==，而且vue也不支持你这样做，因为这会让你的vue实例代码变得臃肿

```html
<template id="temp">
    <div>
        <h2>{{title}}</h2>
        <p>内容</p>
    </div>
</template>
```

```javascript
components:{
    "cpn1": {
        template: "#temp",
            // 这里要求data必须是一个函数，且必须return一个对象，对象的内容就是它的数据 语法糖简写为此
            data(){
            return {
                title: 'haha'
            }
        }
    }
}
```

vue的组件跟vue实例很相似，vue实例拥有的属性和功能组件大多数拥有

### 6.7、父子组件的通信

#### 1、父传子

在vue中父组件向子组件传递数据需要使用==props==

以下实例中我会把vue实例当做父组件使用

```html
<div id="app">
    <cpn1 :title="title"></cpn1> <!-- 这里选择绑定的属性，使用 :属性名-->
</div>
<template id="temp">
    <div>
        <h2>{{title}}</h2>  <!-- 这里再取-->
        <p>内容</p>
    </div>
</template>
```

```javascript
const app = new Vue({
    el:"#app",
    data:{
        title: '父组件值'
    },

    components:{
        "cpn1": {
            template: "#temp",
            props: ['title'], // 传递为一个数组
            data(){
                return {} // 如果直接选择使用父组件的数据的话 此出就可以返回空了
            }
        }
    }
})
```

上述的例子中我们传入的是一个数组类型，还有很多类型供选择

例：对象类型{} ，boolean，String，Number等

扩展实例：

```html
<div id="app">
    <cpn1 :title="title" :message="message"></cpn1> <!-- 这里选择绑定的属性，使用 :属性名-->
</div>
<template id="temp">
    <div>  <!-- 这里有一个小细节，当标签多的时候，这里要有个根标签来包裹着所有标签-->
        <h2>{{title}} ++ {{message}}</h2>  <!-- 这里再取-->
        <p>内容</p>
    </div>
</template>
```

```javascript
const app = new Vue({
		el:"#app",
		data:{
			title: '父组件值',
			message: '111'
		},
		
		components:{
			"cpn1": {
				template: "#temp",
				props:{
					// 此处为一个对象
					title: String, // 类型为string
					message: {
						type: String, // 类型为string
						default: "22222222", // 默认值为此
						required: true // 必须穿此值
					}
				},
				data(){
					return {} // 如果直接选择使用父组件的数据的话 此出就可以返回空了
				}
			}
		}
	})
```

props引起的驼峰问题

```html
<cpn1 :child-parent="childParent"></cpn1> 此处的驼峰不识别，必须为 - 隔开
```

```javascript
const app = new Vue({
    el:"#app",
    data:{
        childParent: ['aaa','bbb','ccc','ddd','eee']
    },

    components:{
        "cpn1": {
            template: "#temp",
            props:['childParent'],
            data(){
                return {} // 如果直接选择使用父组件的数据的话 此出就可以返回空了
            }
        }
    }
})
```

#### 2、子传父

子组件向父组件传递数据时需要我们自定义方法实现

```html
<div id="app">
    <cpn1 @btnclick="btnClick"></cpn1>  <!--此处的@xxx不能是驼峰，如果有大写，就用 ‘-’隔开，要与下面自己定义的事件名相同-->
</div>
<template id="temp">
    <div>
        <h2></h2> 
        <button type="button" @click="btnClick(content)">{{content}}</button>
        <p>内容</p>
    </div>
</template>
```

```javascript
const app = new Vue({
    el:"#app",
    data:{
    },
    methods:{
        btnClick(event){
            console.log("只能拿到事件")
        }
    },
    components:{
        "cpn1": {
            template: "#temp",
            data(){
                return {
                    content: '奶粉'
                }
            },
            methods:{
                btnClick(content){
                    this.$emit('btnclick'); // 此处是自定义事件名，代表发射到父组件上
                }
            }
        }
    }
})
```

==63-71没看==

## 7、模块化

### 7.1、模块化简介

​	前端模块化的思想萌生，原因是因为js文件越来越多，代码量越来越多，整合js文件时限制太大，所以就提出了模块化的思想，其实使用匿名函数也可以解决，但是匿名函数一旦定义，则不能在全局使用，因此，模块化的开发迅速流行了起来

### 7.2、ES6模块化开发

使用Es6进行模块化开发需要用到两个关键字: ==export==   ==import==

例：

以下代码来自index.html

```html
<script src="../js/index.js" type="moudel" charset="utf-8"></script> <!--使用 关键字来表明这是一个模块，用于后续开发-->
<script src="../js/demo.js" type="moudel" charset="utf-8"></script>
```

以下代码来自main.js

```javascript
// 常规变量
const name = 'aaa';
const address = "上海";
const bool = false;

// 函数
function sum ( num1, num2){
	return num1 + num2;
}
if(bool){
	console.log(sum(20,30));
}
// 类
class Person{
	run(){
		return "在跑";
	}
}
// 默认值 允许调用者自己起变量名 default修饰的变量或者函数，必须全局唯一
export default function (variable){
	console.log(variable)
}
// 导出语法一共两种，
// 第一种为：直接使用定义好的所有的变量名，类名，方法名进行
export {
	name,address,bool,sum,Person
}
// 第二种为：现场定义然后马上导出
export const a = "bbb"
```

以下代码来自demo.js

```javascript
// 导入语法
import {a,name,sum,Person} from "./index.js" // 就可以使用了
console.log(a);
console.log(name)
console.log(sum(500,1000));
let p = new Person();
console.log(p.run())


import vari from './index.js' // 导入default修饰的
vari("xxx")

// import 所有
import * as /*起别名*/ temp from "./index.js" 

console.log(temp.name + temp.a)
```

## 8、webpack

### 8.1、webpack简介

它是一个JavaScript的静态==模块打包==工具，其他的请自行百度

### 8.2、webpack安装

webpack运行依赖node.js，因此要安装node.js

使用node.js的工具 npm命令安装webpack

```shell
npm install webpack@3.6.0 -g # 全局安装
```

安装完成之后，查看版本

```shell
webpack -v
```

### 8.3、webpack起步

#### 8.3.1、前端工程目录结构

dist：打包之后代码存放的地方 ，是准备发布的目录

src：是源码存放的地方

main.js/index.js ：代表一个前端js程序的入口

#### 8.3.2、webpack基本使用

```shell
cd 到指定文件位置处后
# 指定打包的文件和生成新的文件的名字
webpack ./src/xx.js ./dist/xxx.js 
```

然后页面再引入打包后的js文件就可以使用了

```html
<script src="./dist/xxx.js" type="module" charset="utf-8"></script>
```

### 8.4、webpack配置文件

指定webpack的一些打包方式和目录，以及目标文件

```javascript
const path = require("path") // 从nodejs导入的
// 创建配置文件，文件名定死为：webpack.config.js

npm init  // 对nodejs初始化 会创建一个package.json

module.exprots={
    entry: "", // 入口
    output:{ // 出口
        path:path.resolve(_dirname,'dist'),// 动态获取
        fiename: 'xxx.js'
    }
}
```

### 8.5、npm方式安装vue

```shell
npm install vue --save
```

## 9、Vue-cli

Vue-cli的安装

```shell
npm install -g @vue/cli
```

使用Vue-cli 3创建项目，目录文件的解析

```shell
vue create 项目名
```

Vue-cli 3创建项目官网有。它依赖的不再是webpack3，而是webpack4，创建项目的具体流程，都可以参照官网

目录解析

```txt
node_modules ：nodejs文件所在的地方
public：静态文件，例如html、icon等
src：源码
.gitignore：排除上传到git的配置文件
babel.config.js：vue的简单的一些基本配置
package.json：vue所依赖的一些文件配置
package-lock.json：vue真实依赖的的那些配置文件的版本
README.md：项目的描述
```

启动Vue-cli3创建的项目

```sh
npm run serve
```

## 10、箭头函数

### 10.1、箭头函数简介和案例

箭头函数是es6的一种对函数的简单写法，**通常我们需要对一个函数进行传参而参数类型是一个函数时，此时我们会考虑使用箭头函数**

下面将列出传统函数与箭头函数的案例和区别

传统函数写法：

```javascript
// 1.1 无返回值无参
// 1.1.1 字面量赋值函数
const obj = {
    aaa(){

    }
}
// 1.1.2 常规定义函数
const a = function(){

}

// 1.2、无返回值有参
const b = function(num1,num2){

}

//1.3 有返回值有参
const c = function(num3,num4){
    return num3 + num4;
}
```

箭头函数写法：

```javascript
// 2.1 无返回值无参
const d = (参数列表) => {

}
// 2.1.1 另一种写法
const d2 = () => {
    console.log("yyy")
}

// 2.2 无返回值有参
const e = (n1,n2) => {

}
// 2.2.1 当只有一个参数时这样写
const e2 = n3 => {

}

// 2.3 有返回值有参
const f = (b1,b2) => {
    return b1 * b2;
}
// 2.3.1 当内部代码块只有一行时，可以这样写
const f2 = (b1,b2) => b1 * b2;
// 或者没有返回值,只有一个参数时可以这样
const f3 = b1 => console.log(b1);
```

现在大家再回头看main.js中的代码是否能够理解它的构造?

```javascript
new Vue({
  render: h => h(App),
}).$mount('#app')
```

通过上述的案例，相信我们对箭头函数的简洁性是不可置疑的，下面我们再来看看箭头函数中的关键地方

### 10.2、箭头函数中的this指向

我们将通过几个案例向你说明this指向的相关问题

例1、

```javascript
// 此处的this指向是？
setTimeout(function(){
    console.log(this)  // Window
},1000);

setTimeout(() => {
    console.log(this) // Window
},1000)
```

例2、

```javascript
const obj = {
    aaa() { 
        setTimeout(function(){
            console.log(this)  // Window
        },1000),

            setTimeout( () => {
            console.log(this)  // aaa()
            // 此处为什么指向了aaa() 这个函数呢？
        },1000)
    }
}
```

例3、

```javascript
const obj = {
    aaa() { 
        setTimeout(function(){
            setTimeout(function(){
                console.log(this)  // Window
            },1000),

                setTimeout(() => {
                console.log(this)  // Window  ???
            },1000)
        },1000),

            setTimeout( () => {
            setTimeout(function(){
                console.log(this)  // Window
            },1000),

                setTimeout(() => {
                console.log(this)  // aaa()  ???
            },1000)
        },1000)
    }
}
```

总结：其实通过上述的例子我们不难看出，this的指向是活动着的，==它引用的其实是临近作用域的对象的this==，这是什么意思呢？就是说假设例1，**setTimeout内部并没有this属性（匿名函数），所以，箭头函数的this朝着外面去查找了一层，发现了this，所以引用它成为自己指向的this，所以就是window**，下面两例也都是再重复这样的动作

## 11、路由

### 11.1、什么是路由

路由（routing）就是通过互联的[网络](https://zh.wikipedia.org/wiki/互聯網)把[信息](https://zh.wikipedia.org/wiki/信息)从源地址传输到目的地址的活动 			---Google

可能这样的说法不是太让人喜欢，那我们试着换种说法表述它的意思

路由：负责传输数据，转送分发

​	1、路由是决定了数据包的来源和转送

​	2、它将输入端的数据转送到合适的输出端

如果还不太理解也没关系，我们将在以下案例和你的深入学习中加深它对你的印象

### 11.2、Vue中的路由

下面是Vue官方对它的介绍，以及对它功能的定义

Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

-   嵌套的路由/视图表
-   模块化的、基于组件的路由配置
-   路由参数、查询、通配符
-   基于 Vue.js 过渡系统的视图过渡效果
-   细粒度的导航控制
-   带有自动激活的 CSS class 的链接
-   HTML5 历史模式或 hash 模式，在 IE9 中自动降级
-   自定义的滚动条行为

### 11.3、起步

#### 11.3.1、安装

```sh
npm install vue-router
# 或者可以使用vue ui命令来进入到ui界面进行插件的安装
vue ui 
添加vue-router插件
# 还可以创建脚手架的时候选择添加 vue-router
(*) vue-router
```

#### 11.2.2、Vue-router基本的使用

以下代码来自index.js

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter) // 导入插件

// 定义路由和组件之间的映射关系
const routes = [
	
]
// 定义路由
const router = new VueRouter({
	routes // es6的简单写法表示引用上边定义的路由
})

// 导出
export default router
```

以下代码来自main.js

```javascript
import router from './router' // 此处是一个简介写法，表示导入此文件夹的index.js文件
new Vue({
	// 引用
  router,
  render: h => h(App),
}).$mount('#app')
```

#### 11.3.3、配置路由和组件的映射

我们先创建两个组件

index.vue

```vue
<template>
	<div id="index">
		<h2>首页</h2>
		<p>HHHHHHHHHHHHHHHHHHHHHHHH</p>
	</div>
</template>

<script>
	export default {
		name: "index"
	}
</script>

<style>
</style>
```

about.vue

```vue
<template>
	<div id="about">
		<h2>关于</h2>
		<p>GGGGGGGGGGGGGGGGGGG</p>
	</div>
</template>

<script>
	export default {
		name: "about"
	}
</script>

<style>
</style>
```

接下来把他导入路由js中

```javascript
// 导入组件
import index from '../components/index.vue'
import about from '../components/about.vue'
```

配置映射关系

```javascript
const routes = [
    // 一个组件和一个路由的映射关系使用一个对象来表示
    {
        path:'/home', // 对应的路径
        component: index // 对应的组件
    },{
        path:'/about',
        component: about
    }
]
```

接下来我们创建两个标签 （==router-link==），当点击他们时，切换路径并渲染页面

最后我们使用 ==router-view== 来渲染组件

```html
<router-link to="/home">首页</router-link>
<br/>
<router-link to="/about">关于</router-link>  <!--router-link 被渲染成a标签，代表路由到的路径-->

<router-view></router-view> <!--router-view决定了视图显示的位置-->
```

#### 11.3.4、默认路径的修改和模式的切换

当我们完成上述例子后发现虽然实现了路径变化的渲染效果，但是刚入浏览器窗口时由于我们没有触发a标签，所以页面并没有被渲染，此时我们考虑将他的默认路径加上

修改默认路径，只需将 路由.js 文件的 映射关系 内部加上 ==重定向== 即可

```javascript
{
    path:'',
    redirect: '/home'
},
```

修改切换模式

url hash的切换模式会使路径加上 # 号，此时我们考虑切换为history模式

```javascript
const router = new VueRouter({
	routes, // es6的简单写法表示引用上边定义的路由
    mode:"history"
})
```

#### 11.3.5、router-link属性补充

```html
<router-link to="/about" replace tag="button" active-class="active">关于</router-link>
<!--replace: 代表点击了标签后不会出现回退，前进的左上角箭头指示-->
<!-- tag: 表示要渲染成目标类型的前端组件 -->
<!-- active-class: 此属性可以修改vuejs对路由加上的默认类名的值，修改多个可以去路由中配置 -->
修改多个的路由配置
const router = new VueRouter({
	routes // es6的简单写法表示引用上边定义的路由
	,
	mode:"history",
	linkActiveClass: ""  <!--指定指定类名即可-->
})
```

#### 11.3.6、代码路由跳转

如果你觉得上述的代码不想使用<router-link> 你可以自己手动的来获得vuejs的路由，然后再操作它也可以完成一样的效果：

```html
<button type="button" @click="H">首页</button>
<button type="button" @click="A">关于</button>
```

```javascript
export default {
    data(){
        return {
            loading: false
        }
    },
    methods: {
        H(){
            this.$router.push("/home");
        },
        A(){
            this.$router.push("/about");
        }
    }
}
```

除此之外，我们还可以使用 $router.replace() 方法来避免带来history记录

```javascript
methods: {
    H(){
        this.$router.replace("/home");
    },
    A(){
        this.$router.replace("/about");
    }
}
```

还有就是我们还提供了 $router.go() 方法来访问不同阶段的历史记录

```javascript
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

#### 11.3.7、动态路由

GitHub example

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const Home = { template: '<div>This is Home</div>' }
const Foo = { template: '<div>This is Foo</div>' }
const Bar = { template: '<div>This is Bar {{ $route.params.id }}</div>' }

const router = new VueRouter({
  mode: 'history',
  base: __dirname,
  routes: [
    { path: '/', name: 'home', component: Home },
    { path: '/foo', name: 'foo', component: Foo },
    { path: '/bar/:id', name: 'bar', component: Bar }
  ]
})

new Vue({
  router,
  template: `
    <div id="app">
      <h1>Named Routes</h1>
      <p>Current route name: {{ $route.name }}</p>
      <ul>
        <li><router-link :to="{ name: 'home' }">home</router-link></li>
        <li><router-link :to="{ name: 'foo' }">foo</router-link></li>
        <li><router-link :to="{ name: 'bar', params: { id: 123 }}">bar</router-link></li>
      </ul>
      <router-view class="view"></router-view>
    </div>
  `
}).$mount('#app')
```

#### 11.3.8、路由嵌套

实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件

接下来我们会通过一个实例向你阐述Vue是如何嵌套的

首先创建两个子组件  news 和  message 认为他们两个是home的 子组件，开始嵌套

```vue
<template>
	<div>
		<ul>
			<li>新闻1</li>
			<li>新闻1</li>
			<li>新闻1</li>
			<li>新闻1</li>
			<li>新闻1</li>
		</ul>
	</div>
</template>

<script>
	export default {
		name: 'news'
	}
</script>

<style>
</style>
另一个一样就不写了
```

接下来我们开始配置路由的js文件

```javascript
// 导入组件
import news from '../components/news.vue'
import message from '../components/message.vue'

// 定义路由
const routes = [
    {
        path:'/home',
        component: index,
        children:[
            {
                path: 'news',
                component: news
            },
            {
                path: 'message',
                component: message
            }
        ]
    }
]
```

接下来我们只需要在父组件的位置处标注上==< router-view >== 和 ==< router-link >== 即可

以下代码来自home.vue

```vue
<!-- 在这里标明子组件的显示 -->
<router-link to="/home/news">新闻</router-link>
<router-link to="/home/message">消息</router-link>
<router-view></router-view>
```

我们也可以在其基础上配置 默认页面

```javascript
{
    path: '',
    redirect: 'news'
}
```

#### 11.3.9、参数传递

##### 11.3.9.1、query传参

以下代码来自App.vue

```vue
<router-link :to="{path:'/profile',query:{name:'xxx',age: 18,height: 1.88}}">profile</router-link>
```

获取值，来自App.vue

```vue
<p>{{$route.query}}</p>
<!--或者-->
<p>
  {{query}}  
</p>
export default {
    data(){
        return {
            query: this.$route.query
        }
    }
}
```

##### 11.3.9.2、监听事件传递数据

我们大部分时候都希望当用户发生某些事件时才传递数据，因此还可以这样来写

当发生点击事件时

以下代码来自App.vue

```vue
<button @click="userClick">用户</button>
<button @click="profileClick">档案</button>
```

```javascript
methods: {
            userClick(){
                this.$router.push("/user/" + this.userid + "/aaa/18")
            },
                profileClick(){
                    this.$router.replace({
                        path:'/profile',
                        query:{
                            name:'xxx',
                            age: 18,
                            height: 1.88
                        }
                    })
                }
}
```

#### 11.3.10、导航守卫

当需要监听路由跳转的过程中我们需要做某些事情时，就可以使用导航守卫

全局前置守卫

```javascript
// 定义路由监听 前置路由
router.beforeEach((to,from,next) => {
	document.title = to.matched[0].meta.title
	next();
})
```

全局解析守卫

​	在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

全局后置钩子

```javascript
router.afterEach((to, from) => {
  // ...
})
```

路由独享守卫

组件内的守卫

`官网自己看`

#### 11.3.11、路由懒加载

js文件太大时，同时加载所有的js需要花费一定时间，所以我们需要将js文件进行懒加载，随用随加载

语法

```javascript
 const index = () => import(/* webpackChunkName: "group-foo" */ '../components/index.vue')
```

#### 11.3.12、route与router的区别

```javascript
this.$router // 拿到的是全局定义的路由对象
this.$route // 拿到的是当前活跃的路由对象
```

#### 11.2.13、Keep-Alive

​	keep-alive是Vue的内置组件，每一次页面的刷新和路由的跳转都会造成以加载后的组件重新创建。当我们将< router-view > 放到keep-alive中时，页面所加载到的所有视图组件都会被缓存

语法：

```vue
<keep-alive>
    <router-view/>
</keep-alive>
```

当使用此标签包裹了视图组件后，此视图将会被缓存，而且此时 activated() 和 deactivated()才会生效，表示页面活跃和页面不活跃时调用

如果我们希望有的视图会被频繁创建和销毁，有的不希望频繁和销毁，我们此时应该这么做

keep-alvie 提供了两个属性

include：当包含所匹配的组件时，此组件会被缓存，默认都是加上的此属性

exclude：当包含所匹配的组件时，此组件不会被缓存 

它们都支持正则表达式

```vue
<keep-alive exclude = "组件name属性的值">
    <router-view/>
</keep-alive>
```

## 12、Vuex

### 12.1、Vuex简介

​	Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 [devtools extension](https://github.com/vuejs/vue-devtools)，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

​	简单的来说，Vuex是一个用来存储公共数据的仓库，当项目足够大型时，有的组件可能需要用到其他跟他毫不相关的组件中的数据，此时如果使用父子组件传值，无疑需要搭建许多重关系，Vue提供Vuex来统一管理这个中心公共仓库的数据

### 12.2、起步

#### 12.2.1、安装

```shell
npm install vuex --save
```

#### 12.2.2、使用插件

项目开发中，我们在根目录创建 store 文件夹，接着在里面写Vuex管理的数据

以下代码来自 store/index.js

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

#### 12.2.3、创建store对象

以下代码来自 store/index.js

```javascript
const store = new Vuex.Store({
    state:{
        counter: 0
    }
});
// 导出
export default store
```

以下代码来自main.js

```javascript
import store from './store'
new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```

自始，你便可以在全局任意地点通过Vuex的$store.state来访问此 counter变量的值

以下代码来自App.vue

```vue
<p>--------这里是App-----------</p>
<div>
    {{$store.state.counter}}
</div>
<button @click="$store.state.counter++">+</button>
<button @click="$store.state.counter--">-</button>
<p>--------这里是helloworld-------------</p>
<hello-world/>
```

但是官方不建议直接使用 $store 操作 Vuex，因为官方做了插件 Devtools来记录通过mutations修改过后的vuex的历史记录，方便开发人员更好的维护和调试，直接修改Vuex是不会被记录的

为此，我们可能需要这样做：

以下代码来自 store/index.js

```javascript
const store = new Vuex.Store({
    state:{
        counter: 0
    },
    mutations:{
        increment(state){
            state.counter ++;
        },
        decrement(state){
            state.counter --;
        }
    }
});
```

以下代码来自App.vue

```vue
<p>--------这里是App-----------</p>
<div>
    {{$store.state.counter}}
</div>
<button @click="add">+</button>
<button @click="sub">-</button>
<p>--------这里是helloworld-------------</p>
<hello-world/>
```

App.vue js

```javascript
methods:{
    add(){
        this.$store.commit("increment"); // 这里提交的是 store/index.js 里的方法，这样Devtools就可以记录修改的状态和记录
    },
        sub(){
            this.$store.commit("decrement");
        }
}
```

### 12.3、Vuex核心概念

#### 12.3.1、State

表示单一状态树，指的是一个Vue项目中建议只使用一个Vuex.Store({}) 来创建仓库对象，多个Store对象会另后期的维护和扩展带来不便，多个Stroe对象也无法保证安全性，因此，只有一个核心Store对象就可以了。

#### 12.3.2、Getters

类似于原本Vue实例中的计算属性的用法，只不过因为他存在于Vuex中，使其被Vuex管理，所有所有的组件都得以轻松访问，而不需要再写冗余的其他代码

下面将通过一个例子来表现Getters的用法

例1、请找出所有的年龄大于20的球员

以下代码来自store/index.js

```javascript
getters:{
    more20stu(state){
        return state.stus.filter(s =>{
            return s.age > 20
        })
        /*箭头函数可以简写为  .filter(s => s.age > 20)*/
    }
}
```

以下代码来自 App.vue

```vue
{{$store.getters.more20stu}}
```

例2、请计算counter的平方为多少

以下代码来自store/index.js

```javascript
squareForCounter(state){
    return state.counter * state.counter
}
```

以下代码来自 App.vue

```vue
{{$store.getters.squareForCounter}}
```

例3、getters传参

以下代码来自store/index.js

```javascript
returnVulueForParam(state){
    return function (age) {
        return state.stus.filter(function (s) {
            return s.age > age;
        })
    }
}
/*简化为箭头函数写法为*/
returnVulueForParam: state => age => state.stus.filter(s => s.age > age)
```

以下代码来自 App.vue

```vue
{{$store.getters.returnVulueForParam(20)}}
```

#### 12.3.3、Actions

因为在mutations中无法使用异步的操作，这样Devtools才能更好的追踪记录，所以我们将所有的异步操作都放入actions中来，然后再将执行后的结果同步提交个mutations

以下代码来自store/index.js

```js
actions:{
    lazyEdit(context,name){
        setTimeout(()=>{
            context.commit("edit",name);
        },1000)
    }
},
    mutations:{
        edit:(state,name) =>{
            state.stus[0].name = name;
        }
    }
```

以下代码来自 App.vue

```js
editStu(name){
    // this.$store.commit("edit",name);
    this.$store.dispatch("lazyEdit",name);
}
```

任何异步的请求都要经过acitons来处理，之后由actions同步提交给mutations即可

#### 12.3.4、Mutations

##### 12.3.4.1、Mutations的带参数传递

Vuex规定更新State时，希望通过Mutations来提交自己的更新，以便于Devtools的追踪记录，基本的使用，上述例子已经阐述过了，下面来看看通过Mutations来传递参数的用法

以下代码来自 App.vue

```javascript
subBtn(num){
    this.$store.commit("decrement",num);
},
addBtn(num){
    this.$store.commit("increment",num);
}
```

以下代码来自store/index.js

```javascript
mutations:{
    increment(state,num){
        state.counter += num;
    },
    decrement(state,num){
        state.counter -= num;
    }
}
```

传递多个参数时，应该这样做

以下代码来自 App.vue

```vue
<button @click="addStu({id:104,name:'xxx',age:30})">addStu</button>
```

传递时为一个对象就可以了，其他的步骤不便，这里的参数官方又叫做==payload==，译为：==载荷==，表示提交时所及附带的内容

##### 12.3.4.2、Mutations的提交风格

```javascript
// 讲普通的提交方式换为payload提交方式
subBtn(num){
    this.$store.commit({
        type: "increment",
        num
    });
},
```

此时，store/index.js那里再取值时，应该使用

```js
state.counter += num.num
```
##### 12.3.4.3、Mutations的常量定义

通过上述些许例子我们发现，如果mutations内的方法名写错，或者当提交时写错方法名，结果都无法正常响应，为此，Vue提供了常量定义，来统一两方的名字问题。具体例子如下：

我们先创建一个mutaions-type.js文件，在里面定义常量

```js
export const DECREMENT = 'decrement';
```

在 store/index.js中导入并使用

```js
import{
    DECREMENT
} from './mutations-type'

mutations:{
    [DECREMENT](state,num){
        state.counter -= num.num;
    }
}
```

在 App.vue中导入并使用

```js
import{
  DECREMENT
} from './store/mutations-type'

subBtn(num){
    this.$store.commit({
        type: DECREMENT,
        num
    });
}
```

#### 12.3.5、Modules

