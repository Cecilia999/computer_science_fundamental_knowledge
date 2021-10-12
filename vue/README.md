# Vue

## this.$router.push 的使用

https://www.huaweicloud.com/articles/d7a66d653e8bf1de0539b33f4d84d457.html

## 初识 Vue:

1. 想让 Vue 工作,就必须创建一个 Vue 实例,且要传入一个配置对象
2. root 容器里的代码依然符合 htm1 规范,只不过混入了一些特殊的 Vue 语法;
3. root 容器里的代码被称为 【vue 模板】;
4. vue 实例和容器是一一对应的
5. 真实开发中只有一个 Vue 实例,并且会配合着组件一起使用;
6. {{xxx}}中的 xxx 要写 js 表达式,且 xxx 可以自动读取到 data 中的所有属性
7. 一旦 data 中的数据发生改变,那么页面中用到该数据的地方也会自动更新
   注意区分:s 表达式和 js 代码(语句)
   1.js 表达式:一个表达式会产生一个值,可以放在任何一个需要值的地方:
   (1). a
   (2). a+b
   (3). demo(1)
   (4). x === y ? 'a' : 'b'
   2.js 代码(语句)
   (1).1f()(}
   (2).for(){}

## 模版语法

1. interpolation 插值
2. **v-bind:id / :id** -->> 引号里的东西会被当作 js 表达式

Vue 模板语法有 2 大类

1. 插值语法:
2. 指令语法: v-??

## 数据绑定

vue 中有 2 种数据绑定的方式:

1. 单向绑定(**v-bind**):数据只能从 data 流向页面
2. 双向绑定(**v-model**):数据不仅能从 data 流向页面,还可以从页面流向 data。
   备注:
   1. 双向绑定一般都应用在表单类元素(form element)上(如: input、 select 等)
   2. **v-model:value** 可以简写为 **v-model**,因为 v-model 默认收集的就是 value 值

## el 与 data 的两种写法

1. el 的两种写法
   (1) new Vue()时候配置 el 属性。

   ```js
   const vm = new Vue({
   	el: '#root',
   	data: {
   		name: 'ming',
   	},
   })
   ```

   (2) 先创建 vue 实例,随后再通过 vm.$mount('#root')指定 el 的值。

   ```js
   vm.$mount('#root') //mount挂载
   ```

2. data 的两种写法
   (1).对象式

   ```
   data: {
   	name: 'ming'
   }
   ```

   (2).函数式

   ```js
   data(){
       return{
           name: 'ming'
       }
   }
   ```

   如何选择:目前哪种写法都可以,以后学习到组件时,data 必须使用函数式,否则会报错

3. 一个重要的原则:
   由 vue 管理的函数, 一定不要写箭头函数, 一旦写了箭头函数, this 就不再是 vue 实例了。

## MVVM(Model-View-viewmodel) model: 模型-视图-视图模型

![alt text](../image/mvvm.jpg)

## 数据代理 data proxy

### 1. 理解 [Object.defineProperty()](https://stackoverflow.com/questions/18524652/how-to-use-javascript-object-defineproperty)

1. enumerable:true 可枚举的，可遍历的，默认值是 false
2. writable:true 控制属性可以被修改，默认值是 false
3. configurable:true 控制属性可以被删除，默认值是 false
4. get(){}
5. set(){}

```js
let number = 18
let person = {
	name: '张三',
	sex: '男',
}

Object.defineproperty(person, 'age', {
	// value: 18,
	// enumerable:true,   //控制属性是否可以枚举,默认值是 false
	// writable:true,     //控制属性是否可以被修改,默认值是 false
	// configurable:true  //控制属性是否可以被別除,默认值是 false

	//当有人读取 person的age属性时,get函数(getter)就会被调用,且返回值就是age的值
	get() {
		console.log('有人读取age属性了')
		return number
	},

	//当有人修改 person的age属性时,set函数(setter)就会被调用,且会收到修改的具体值
	set(value) {
		console.log('有人修改了age属性,且值是', value)
		number = value
	},
})
```

### 2. 理解数据代理

```js
let obj = { x: '100' }
let obj2 = { y: '200' }

Object.defineProperty(obj2, 'x', {
	get() {
		return obj.x
	},
	set(value) {
		obj.x = value
	},
})
```

### 3. Vue 中的数据代理 Proxy -->> vm.\_data

![alt text](../image/vue的数据代理.jpg)

```js
function proxy(target, sourceKey, key) {
	Object.defineProperty(target, key, {
		get() {
			return this[sourceKey][key]
		},

		set(val) {
			this[sourceKey][key] = val
		},
	})
}
```

## Event Handling 事件处理

### 1. 事件的基本使用 v-on:click

```js
<div class='root'>
    <button @click="showInfo1">点我(不传参数)</button>
    <button v-on:click="showInfo2(66, $event)">点我(传参数)</button>
</div>

<script type="text/javascript">
    const vm = new Vue({
        el: '#id',
        data(){
            name: 'ming'
        },
        //!!!!!!!!!!!
        methods(){
            showInfo1(event){
                alert("hello world")
                console.log(this) //此处this 就是vm
                console.log(event.target.innerText) //event就是这个点击button这个动作
            },
            showInfo2(number, event){
                alert("hello world")
                console.log(number) //66
                console.log(event) //event就是这个点击button这个动作
            }
        }
    })
</script>
```

**事件的基本使用:**

1. 使用 v-on:xxx 或@xxx 绑定事件,其中 xxx 是事件名
2. 事件的回调需要配置在 methods 对象中,最终会在 vm 上;
3. methods 中配置的函数,不要用箭头函数!否则 this 就不是 vm 而是 window 了
4. methods 中配置的函数,都是被 vue 所管理的函数, this 的指向是 vm 或组件实例对象
5. @click="demo"和 @click="demo($event)”效果一,但后者可以传参

### 2. [Event Modifiers 事件修饰符](https://vuejs.org/v2/guide/events.html#Event-Modifiers)

**event capture: 捕获事件**  
**event propagation: 冒泡事件**

vue 中的事件修饰符:

1. prevent: 阻止默认事件(常用) prevent default event, 比如 @scroll 不滚动
2. stop: 阻止事件冒泡(常用) the click event's propagation will be stopped
3. once: 事件只触发一次(常用)
4. capture: 使用事件的捕获模式
5. se1f: 只有 event.target 是当前操作的元素时才触发事件, 也可以阻止 propagation
6. passive: 事件的默认行为立即执行,无需等待事件回调执行完毕; 比如滚动：@wheel

### 3. [Key Modifiers 按键修饰符](https://vuejs.org/v2/guide/events.html#Key-Modifiers)

**key event 按键事件(常用)：**

1. @keyup: 按下去松手触发
2. @keydown: 按下去就触发

**key modifier**

键盘上的每个键都有自己的名字和编码

- event.key 名字
- event.keycode 编码

1. if(event.keycode!==13) return **===** @keyup.enter
2. **@keydown.tab(特殊，必须配合 keydown)**: tab 键本身的作用就是把焦点从当前元素移走，所以不适合用 keyup，因为按键还没抬起来焦点已经从当前元素移走了
3. .delete (captures both “Delete” and “Backspace” keys)
4. .esc
5. .space
6. .up
7. .down
8. .left
9. .right
10. 其他的使用姓名或者编号判断，但是多个单词组成的如 CapsLock -->> @keyup.caps-lock

**总结**

1. key modifiers
2. Vue 未提供別名的按键,可以使用按键原始的 key 值去绑定,但注意要转为 kebab-case(短横线命名)
3. **系统修饰键(用法特殊)**: **ctrl、alt、shift、command**
   (1),配合 keyup 使用: 按下修饰键的同时,再按下其他键,随后释放其他键,事件才被触发
   (2),配合 keydown 使用: 正常触发事件
4. 也可以使用 keyCode 去指定具体的按键(不推荐), 用键名
5. **Vue.config.keyCodes.自定义键名 = 键码**, 可以去定制按健别名 (不推荐)

### 4. 事件总结

修饰符可以连续写，顺序 matters

1. @click.stop.prevent: 先阻止冒泡再阻止默认行为，order matters
2. @keyup.ctrl.y

## 姓名案例：interpolation vs methods

只要 data 中的数据发生改变，vue 一定会重新解析模版<template></template> -->> **re-render**

## [Computed Properties and Watchers 计算属性和监视属性](https://vuejs.org/v2/guide/computed.html)

### 1. computed - 计算属性

1. 定义:要用的属性不存在,要通过已有属性(**data:{}中的数据就是属性，property**)计算得来
2. 原理:底层借助了 Objet.defineproperty 方法提供的 getter 和 setter
3. get 函数什么时候执行?
   (1). 初次读取时会执行一次
   (2). 当依赖的数据发生改变时会被再次调用
4. 优势: computed properties are cached based on their reactive dependencies. A computed property will only re-evaluate when some of its reactive dependencies have changed. In comparison, a method invocation will always run the function whenever a re-render happens. 与 methods 实现相比, 内部有缓存机制(复用), 效率更高, 调试方便。
5. 备注
   - 计算属性(**computed:{}**)最终会出现在 vm 上,直接读取使用即可
   - 如果计算属性要被修改,那必须写 set 函数去响应修改,且 set 中要引起计算时依赖的数据发生
   - 如果只监听不修改可以简写

```js
new Vue({
    data:{
        firstName: 'Ming',
        lastName: 'Chen'
    },
    computed:{
        fullName1:{
            get(){
                return this.firstName + "-" + this.lastName;
            }
            set(value){

            }
        },

        fullName2(){
            return this.firstName + "-" + this.lastName;
        }
    }
})
```

### 2. watch - 监视属性

1. 当被监视的属性变化时,回调函数自动调用,进行相关操作
2. 监视的属性必须存在,才能进行监视!
3. 监视的两种写法
   (1)new Vue({})时传入 watch 配置
   (2)通过 vm.$watch 监视

```js
<script>
const vm = new Vue({
    data:{
        isHot: true;
    },
    computed:{
        info(){
            return this.isHot = !this.isHot;
        }
    },
    watch:{
        isHot:{
            immediate:true, //初始化是让handler调用一下
            handler(newVal, oldVal){
                console.log("isHot被修改了", newVal, oldVal)
            }
        },
        info:{
            handler(){
                console.log("info is changed")
            }
        }
    }
})

vm.$watch.('isHot', {
    immediate:true, //初始化是让handler调用一下
    handler(newVal, oldVal){
        console.log("isHot被修改了", newVal, oldVal)
    }
})
</script>
```

**深度监视:**
(1) Vue 中的 watch 默认不监测对象内部值的改变(一层)
(2) 配置 **deep:true** 可以监测对象内部值改变(多层)
**备注**
(1) Vue 自身可以监测对象内部值的改变,但 Vue 提供的 watch 默认不可以
(2) 使用 watch 时根据数据的具体结构,决定是否采用深度监视

```js
<script>
const vm = new Vue({
    data:{
        isHot: true;
        numbers:{
            a: 1;
            b: 1;
        }
    },
    watch:{
        //监测多级结构中某个属性的变化
        'numbers.a':{
            handler(){
                console.log("a is changed")
            }
        },
        //监测多级结构中所有属性的变化
        numbers:{
            deep: true,  //深度监视
            handler(){
                console.log("numbers is changed")
            }
        },
        //当配置项只需要handler的时候可以简写
        isHot(newVal, oldVal){
            console.log("isHot is changed", newVal, oldVal)
        }
    }

    //当配置项只需要handler的时候可以简写
    //不能写成箭头函数
    vm.$watch('isHot', function(newVal, oldVal){
        console.log("isHot is changed", newVal, oldVal)
    })
})
</script>
```

### 3. computed vs watch

computed 和 watch 之间的区别

1. computedi 能完成的功能, watch 都可以完成。
2. watch 能完成的功能, computed 不一定能完成,例如: **watch 可以进行异步操作** 两个重要的小原则:
   (1) 所被 vue 管理的函数,最好写成普通函数, 这样 this 的指向才是 vm 或组件实例对象
   (2) **所有不被 vue 所管理的函数(定时器的回调函数、ajax 的回调函数等),最好写成箭头函数, 这样 this 的指向才是 vm 或组件实例对象**

## [Class and Style Bindings - Class 与 Style 绑定](https://vuejs.org/v2/guide/class-and-style.html)

### 1. Binding HTML class

1. Object Syntax
2. Array Syntax
3. With Components

```html
<!-- 假设有七种样式 -->
<style>
.basic{}
.happy{}
.sad{}
.normal{}
.test1{}
.test2{}
.test3{}
</style>

<body>
    <div id='root'>
        <!--1. 绑定c1ass样式--字符串写法,适用于:样式的类名不确定,需要动态指定-->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div>

        <!--2. 绑定c1ass样式--数组写法,适用于:要绑定的个数不确定,名字也不确定-->
        <div class="basic" :class="classArr" @click="changeMood">{{name}}</div>

        <!--3. 绑定c1ass样式--对象写法,适用于:要绑定的个数确定,名字也确定，但要动态决定用不用-->
        <div class="basic" :class="classObj" @click="changeMood">{{name}}</div>
    </div>
</body>

<script>
    const vm = new Vue({
        el: '#root',
        data:{
            name: 'ming',
            mood: 'normal',
            classArr: ['test1', 'test2', 'test3'],
            classObj:{
                //默认都不用
                test1: false,
                test2: flase
            }
        },
        methods:{
            //随机切换三种心情
            changeMood(){
                const moods = ['happy', 'sad', 'normal']
                int index = Math.floor(Math.random()*3)
                return this.mood = moods[index];
            }
        }
    })
</scirpt>
```

### 2. [Binding inline styles](https://vuejs.org/v2/guide/class-and-style.html#Binding-Inline-Styles)

## [Conditional rendering 条件渲染](https://vuejs.org/v2/guide/conditional.html)

1. **v-show=**: The difference is that an element with v-show will always be rendered and remain in the DOM; v-show only toggles the **display CSS property** of the element.
2. **v-if**: v-if is used to toggle the existence of the element
3. **v-else**: A v-else element must immediately follow a **v-if or a v-else-if** element - otherwise it will not be recognized.
4. template 只能配合 v-if，不能配合 v-show: template serves as a invisable wrapper
5. [Controlling Reusable Elements with key](https://vuejs.org/v2/guide/conditional.html#Controlling-Reusable-Elements-with-key)
6. [v-if vs v-show](https://vuejs.org/v2/guide/conditional.html#v-if-vs-v-show)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8/" />
		<title>条件渲染</title>
		<script type="text/javascript" src="./is/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<!--使用v-show做条件渲染-->
			<h2 v-show="false">欢迎来到{{name}}</h2>
			<h2 v-show="1===1">欢迎来到{{name}}</h2>

			<!--使用v-if做条件渲染-->
			<h2 v-1f=" false">欢迎来到{{name}}</h2>
			<h2 v-if="1===1">欢迎来到{{name}}</h2>

			<!--v-else 和 v-else-if-->
			<div v-if="n === 1">Angular</div>
			<div v-else-if="n===2">React</div>
			<!-- <div>@</div>   不允许中间被打断 -->
			<div v-else-if="n===3">Vue</div>
			<div v-else>哈哈</div>
		</div>
	</body>
</html>
```

## [List rendering](https://vuejs.org/v2/guide/list.html)
