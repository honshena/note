# Vue

# 安装

vue:[官网文档](https://v3.cn.vuejs.org/guide/installation.html#%E5%8F%91%E5%B8%83%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E)

vue-cli: [官网文档](https://cli.vuejs.org/zh/guide/)

脚手架: `npm install -g @vue/cli`

[脚手架配置](https://maomin.blog.csdn.net/article/details/103252533?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control)

创建项目: `vue create 项目名称` 或使用脚手架2的方法`vue init webpack 项目名称`

可视化管理: `vue ui`

使用ts:`vue add typescript`

# 启动

​	`npm run serve `

# 构建

​	`npm run build`

# 语法

## 基础

```js
//循环
<div v-for="变量 in 属性" v-bind:key="唯一key">{{变量}}</div>
//数组遍历
<div v-for="(item,index) in 属性" v-bind:key="唯一key">{{变量}}</div>
//对象遍历
<div v-for="(value,key) in 属性" v-bind:key="唯一key">{{变量}}</div>

//只渲染变量的初始值,变量更改后视图不更新
<div v-once>{{变量}}</div>

//解析html
<div v-html="<div></div>"></div>

//显示文本
<div v-text="123">此处的文本不会显示,会被v-text内容替换</div>

//不解析vue相关原样显示
<div v-pre>此处的变量不会显示{{变量}}</div>

//和style一起使用不显示未解析变量前的{{变量}}js解析完成后该属性消失
<div v-cloak>此处的变量在加载完成后才会显示{{变量}}</div>
<style>
    [v-clock]{
        display:'none'
    }
</style>

//动态绑定属性
<img v-bind:src="动态变量"/>
//动态绑定类名--对象 普通的class和动态绑定calss会合并
<img v-bind:class="{类名:布尔变量,类名:布尔变量}" class="类名"/>
//动态绑定类名--数组
<img :class="[变量,变量]"/>    
//动态绑定样式--对象
 <img :style="{属性名: 属性变量值,属性名: 属性变量值}"/>
//动态绑定样式--数组
<img :style="[样式对象变量,样式对象b]"/>

//计算属性: 某些变量需要合在一起等,会缓存
<div>{{fullName}}</div>
computed:{
    fullName: {
        set: function(preValue){
      		//一般不用
        }
        get: function(){
      		return this.firstName + ' ' + this.lastName
        }
    }
},
//如果没有set方法则计算属性可以简写
 computed:{
    fullName: function(preValue){
      		return this.firstName + ' ' + this.lastName
    }
}

//判断
<div v-if="true"></div>
<div v-else-if="false"></div>
<div v-else-if="false"></div>
<div v-else></div>
//显示元素,v-if会卸载元素,v-show通过css的display:none隐藏元素
<div v-show="true"></div>
//虚拟dom复用,加入属性key后如果key不一样则不会复用
<input key="1" v-if="show" id="Input1"/>
<input key="2" v-else id="Input2"/>
    
//过滤器
<div>{{list|odd}}}</div>
filters:{
    odd(num){
      return num%2?1:0
    }
  },
  data: function() {
return {
    list: [1,2,3],
  };
      
//watch
data(){
   return {
     num: 0
   }
  },
watch:{
	num(){
	//函数名和监视的data中的变量名一致,当data中的变量名改变时会触发watch中的函数执行
    }
}
```

## 事件

```js
//点击获取事件源 
<div v-on:click="函数名"></div> //默认参数为event
<div v-on:click="函数名(参数)"></div>
<div @click="函数名(参数,$event)"></div>
//使用修饰符阻止事件冒泡
<div @click.stop="add"></div> //相当于event.stopPropagation
<div @click.prevent="add"></div> //相当于event.preventDefault
<div @click.stop.prevent="add"></div> //串联修饰符
//监听指定按键回调
<div @keyup.13="add"></div> 
<div @keyup.enter="add"></div> 
//只触发一次回调
<div @click.on="add"></div> 
//监听根元素的原生组件
<my @click.native="add"></my> 
```

# 生命周期

`beforeCreate` : 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
`created`:  在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，property 和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el property 目前尚不可用。
`beforeMount` :  在挂载开始之前被调用：相关的 render 函数首次被调用。该钩子在服务器端渲染期间不被调用

`mounted`: 实例被挂载后调用，这时 [`app.mount`](https://v3.cn.vuejs.org/api/application-api.html#mount) 被新创建的 `vm.$el` 替换了。如果根实例挂载到了一个文档内的元素上，当 mounted 被调用时 `vm.$el` 也在文档内。

`beforeUpdate` : 数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。**该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。**updated
`updated`:  由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用[计算属性](https://v3.cn.vuejs.org/api/options-data.html#computed)或[侦听器](https://v3.cn.vuejs.org/api/options-data.html#watch)取而代之。

`activated`:  被 keep-alive 缓存的组件激活时调用。**该钩子在服务器端渲染期间不被调用**

`deactivated `: 被 keep-alive 缓存的组件停用时调用。

` beforeUnmount`: 在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的。**该钩子在服务器端渲染期间不被调用。**

`unmounted`: 卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载。**该钩子在服务器端渲染期间不被调用。**

`errorCaptured`: 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 `false` 以阻止该错误继续向上传播。

`renderTracked`: 跟踪虚拟 DOM 重新渲染时调用。钩子接收 `debugger event` 作为参数。此事件告诉你哪个操作跟踪了组件以及该操作的目标对象和键。

`renderTriggered`: 当虚拟 DOM 重新渲染被触发时调用。和 [`renderTracked`](https://v3.cn.vuejs.org/api/options-lifecycle-hooks.html#rendertracked) 类似，接收 `debugger event` 作为参数。此事件告诉你是什么操作触发了重新渲染，以及该操作的目标对象和键。

![生命周期](https://images-1300732204.cos.ap-chengdu.myqcloud.com/lifecycle.svg)

# 双向数据绑定

```js
//输入框实现双向数据绑定
<input v-model="phone" />
<div>{{phone}}</div>
//输入框实现双向数据绑定--以前写法
<input :value="phone" oninput="changePhone"/>
<div>{{phone}}</div>
changePhone (e){
    this.phone=e.target.value
}

//单选框实现双向数据绑定
<label for="male">
      <input v-model="sex" value="male" id="male" type="radio">男
</label>
<label for="female">
      <input v-model="sex" value="female" id="female" type="radio">女
</label>
<div>选择结果是: {{sex}}</div>

//多选框实现双向数据绑定
<input v-model="sport" value="篮球" type="checkbox">篮球
<input v-model="sport" value="羽毛球" type="checkbox">羽毛球
<div>选择结果是: {{sport}}</div>

//select单选绑定
 <select v-model="sport" name="sport">
      <option  value="篮球" >篮球</option>
      <option  value="羽毛球" >羽毛球</option>
</select>
 <div>选择结果是: {{sport}}</div>

//select多选绑定
 <select v-model="sport" name="sport" multiple>
      <option  value="篮球" >篮球</option>
      <option  value="羽毛球" >羽毛球</option>
</select>
 <div>选择结果是: {{sport}}</div>
```

**不要在子组件中使用双向绑定修改父组件的值**

## 修饰符

```js
.lazy: <input v-model.lazy="phone"/> //在输入框失去焦点或回车后将绑定数据更新,而不是实时改变
.number: <input v-model.number="phone"/> //一般v-model绑定数据是将input中的value当做string类型,修饰后只更新数据则是number类型
.trim: //去除输入的空格
```

# 模板

```js
//方式一
<script type="text/x-template" id="myComponent">
    <div>
    	自定义组件
    </div>
</script> 
//方式二
<template id="myComponent">
    <div>
    自定义组件
    </div>
</template>
//id为模板使用时可以获取的
```

# 插槽

`slot`

```js
<my>
    自定义组件中的内容在此处将被收集到插槽中,类似react,此处如果有多个标签则多个标签都会替换到slot中
</my>
//默认值`
<solt>此处可以放默认值,不传时显示此处</slot>
```

## 具名插槽

```js
//当自定义组件中含有多个slot时,需要替换指定的插槽
<template>
    <slot name="left"></slot>
   <slot name="right"></slot>
   <slot></slot>
</template>
//替换时如果不写名字则全部替换
<myComponent>
       <div>自定义组件中所有没有name属性的slot标签都将被替换</div>
</myComponent>  
//替换指定位置
<myComponent>
       <div slot="right">自定义组件中name属性为right的slot标签都将被替换</div>
</myComponent>
```

# 父子组件通信

## props

```js
//当属性是对象何数组时默认值是函数
props:{
    list:{
        type: Array,
        default(){
            return []
        }
    }
}
```



## 事件传递

```js
//在子组件的函数中使用
methods: {
    func(){
        this.$emit('父组件绑定自定义事件',参数)
    }
}
//父组件,此时默认的事件对象是子组件传递的参数而不是event
< @自定义事件名='函数' />
```

## 直接访问

```js
//通过 this.$children 访问子组件
//this.$parent 访问父组件不建议这样使用
//this.$root 访问根组件
//父组件通过this.$refs访问,子组件需要有属性名ref="唯一id"
```

# 事件总线

```js
//app.vue
vue.prototype.$bus=new Vue()
//组件1 监听事件
this.$bus.$on('监听自定义事件名',()=>{}) //回调函数
//组件2 派发事件
this.$bus.$emit('自定义事件名')
```

# 混入(mixin)

Mixin 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个 mixin 对象可以包含任意组件选项。当组件使用 mixin 对象时，所有 mixin 对象的选项将被“混合”进入该组件本身的选项。

例子：

```js
// define a mixin object
const myMixin = {
  created() {
    this.hello()
  },
  methods: {
    hello() {
      console.log('hello from mixin!')
    }
  }
}

// define an app that uses this mixin
const app = Vue.createApp({
  mixins: [myMixin]
})

app.mount('#mixins-basic') // => "hello from mixin!"
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18

## [#](https://v3.cn.vuejs.org/guide/mixins.html#选项合并)选项合并

当组件和 mixin 对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

比如，每个 mixin 可以拥有自己的 `data` 函数。每个 `data` 函数都会被调用，并将返回结果合并。在数据的 property 发生冲突时，会以组件自身的数据为优先。

```js
const myMixin = {
  data() {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  data() {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created() {
    console.log(this.$data) // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21

同名钩子函数将合并为一个数组，因此都将被调用。另外，mixin 对象的钩子将在组件自身钩子**之前**调用。

```js
const myMixin = {
  created() {
    console.log('mixin 对象的钩子被调用')
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  created() {
    console.log('组件钩子被调用')
  }
})

// => "mixin 对象的钩子被调用"
// => "组件钩子被调用"
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
const myMixin = {
  methods: {
    foo() {
      console.log('foo')
    },
    conflicting() {
      console.log('from mixin')
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  methods: {
    bar() {
      console.log('bar')
    },
    conflicting() {
      console.log('from self')
    }
  }
})

const vm = app.mount('#mixins-basic')

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28

## [#](https://v3.cn.vuejs.org/guide/mixins.html#全局-mixin)全局 mixin

你还可以为 Vue 应用程序全局应用 mixin：

```js
const app = Vue.createApp({
  myOption: 'hello!'
})

// 为自定义的选项 'myOption' 注入一个处理器。
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

app.mount('#mixins-global') // => "hello!"
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15

Mixin 也可以进行全局注册。使用时格外小心！一旦使用全局 mixin，它将影响**每一个**之后创建的组件 (例如，每个子组件)。

```js
const app = Vue.createApp({
  myOption: 'hello!'
})

// 为自定义的选项 'myOption' 注入一个处理器。
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

// 将myOption也添加到子组件
app.component('test-component', {
  myOption: 'hello from component!'
})

app.mount('#mixins-global')

// => "hello!"
// => "hello from component!"
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23

大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为[插件](https://v3.cn.vuejs.org/guide/plugins.html)发布，以避免重复应用 mixin。

## [#](https://v3.cn.vuejs.org/guide/mixins.html#自定义选项合并策略)自定义选项合并策略

自定义选项在合并时，默认策略为简单地覆盖已有值。如果想让某个自定义选项以自定义逻辑进行合并，可以在 `app.config.optionMergeStrategies` 中添加一个函数：

```js
const app = Vue.createApp({})

app.config.optionMergeStrategies.customOption = (toVal, fromVal) => {
  // return mergedVal
}
```

1
2
3
4
5

合并策略接收在父实例和子实例上定义的该选项的值，分别作为第一个和第二个参数。让我们来检查一下使用 mixin 时，这些参数有哪些：

```js
const app = Vue.createApp({
  custom: 'hello!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => {
  console.log(fromVal, toVal)
  // => "goodbye!", undefined
  // => "hello", "goodbye!"
  return fromVal || toVal
}

app.mixin({
  custom: 'goodbye!',
  created() {
    console.log(this.$options.custom) // => "hello!"
  }
})
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17

如你所见，在控制台中，我们先从 mixin 打印 `toVal` 和 `fromVal`，然后从 `app` 打印。如果存在，我们总是返回 `fromVal`，这就是为什么 `this.$options.custom` 设置为 `hello!` 最后。让我们尝试将策略更改为*始终*从子*实例*返回值：

```js
const app = Vue.createApp({
  custom: 'hello!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => toVal || fromVal

app.mixin({
  custom: 'goodbye!',
  created() {
    console.log(this.$options.custom) // => "goodbye!"
  }
})
```

## [#](https://v3.cn.vuejs.org/guide/mixins.html#不足)不足

在 Vue 2 中，mixin 是将部分组件逻辑抽象成可重用块的主要工具。但是，他们有几个问题：

- Mixin 很容易发生冲突：因为每个 mixin 的 property 都被合并到同一个组件中，所以为了避免 property 名冲突，你仍然需要了解其他每个特性。
- 可重用性是有限的：我们不能向 mixin 传递任何参数来改变它的逻辑，这降低了它们在抽象逻辑方面的灵活性。

# Provide / Inject

> 本指南假定你已经阅读了 [Provide / Inject](https://v3.cn.vuejs.org/guide/component-provide-inject.html)、[组合式 API 介绍](https://v3.cn.vuejs.org/guide/composition-api-introduction.html)和[响应性基础](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html)。如果你不熟悉组合式 API，请先阅读这篇文章。

我们也可以在组合式 API 中使用 [provide/inject](https://v3.cn.vuejs.org/guide/component-provide-inject.html)。两者都只能在当前活动实例的 [`setup()`](https://v3.cn.vuejs.org/guide/composition-api-setup.html) 期间调用。

## [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#设想场景)设想场景

假设我们要重写以下代码，其中包含一个 `MyMap` 组件，该组件使用组合式 API 为 `MyMarker` 组件提供用户的位置。

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  provide: {
    location: 'North Pole',
    geolocation: {
      longitude: 90,
      latitude: 135
    }
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21

```vue
<!-- src/components/MyMarker.vue -->
<script>
export default {
  inject: ['location', 'geolocation']
}
</script>
```

1
2
3
4
5
6

## [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#使用-provide)使用 Provide

在 `setup()` 中使用 `provide` 时，我们首先从 `vue` 显式导入 `provide` 方法。这使我们能够调用 `provide` 来定义每个 property。

`provide` 函数允许你通过两个参数定义 property：

1. name (`<String>` 类型)
2. value

使用 `MyMap` 组件后，provide 的值可以按如下方式重构：









 









 

 

 

 

 

 

 





```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide } from 'vue'
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  setup() {
    provide('location', 'North Pole')
    provide('geolocation', {
      longitude: 90,
      latitude: 135
    })
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22

## [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#使用-inject)使用 inject

在 `setup()` 中使用 `inject` 时，也需要从 `vue` 显式导入。导入以后，我们就可以调用它来定义暴露给我们的组件方式。

`inject` 函数有两个参数：

1. 要 inject 的 property 的 name
2. 默认值 (**可选**)

使用 `MyMarker` 组件，可以使用以下代码对其进行重构：





 





 

 

 

 

 

 

 

 

 





```vue
<!-- src/components/MyMarker.vue -->
<script>
import { inject } from 'vue'

export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')

    return {
      userLocation,
      userGeolocation
    }
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16

## [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#响应性)响应性

### [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#添加响应性)添加响应性

为了增加 provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 [ref](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#创建独立的响应式值作为-refs) 或 [reactive](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#声明响应式状态)。

使用 `MyMap` 组件，我们的代码可以更新如下：









 









 

 

 

 

 

 

 

 







```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    provide('location', location)
    provide('geolocation', geolocation)
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25

现在，如果这两个 property 中有任何更改，`MyMarker` 组件也将自动更新！

### [#](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#修改响应式-property)修改响应式 property

当使用响应式 provide / inject 值时，**建议尽可能将对响应式 property 的所有修改限制在\*定义 provide 的组件\*内部**。

例如，在需要更改用户位置的情况下，我们最好在 `MyMap` 组件中执行此操作。





























 

 

 

 

 





```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    provide('location', location)
    provide('geolocation', geolocation)

    return {
      location
    }
  },
  methods: {
    updateLocation() {
      this.location = 'South Pole'
    }
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34

然而，有时我们需要在注入数据的组件内部更新 inject 的数据。在这种情况下，我们建议 provide 一个方法来负责改变响应式 property。























 

 

 





 







```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    const updateLocation = () => {
      location.value = 'South Pole'
    }

    provide('location', location)
    provide('geolocation', geolocation)
    provide('updateLocation', updateLocation)
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30











 







 







```vue
<!-- src/components/MyMarker.vue -->
<script>
import { inject } from 'vue'

export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')
    const updateUserLocation = inject('updateLocation')

    return {
      userLocation,
      userGeolocation,
      updateUserLocation
    }
  }
}
</script>
```

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18

最后，如果要确保通过 `provide` 传递的数据不会被 inject 的组件更改，我们建议对提供者的 property 使用 `readonly`。









 



















 

 







```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, readonly, ref } from 'vue'
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    const updateLocation = () => {
      location.value = 'South Pole'
    }

    provide('location', readonly(location))
    provide('geolocation', readonly(geolocation))
    provide('updateLocation', updateLocation)
  }
}
</script>
```

# 编译作用域

```js
//子组件的属性变量会在实例中查找,此处的item会在父组件中查找
<myComponent>
       <div slot="right">{{item}}</div>
</myComponent>
//若item是子组件的data,需要在插槽上动态绑定自定义属性
<template>
   <slot name="left"></slot>
   <slot name="right" :data="item"></slot>
   <slot></slot>
</template>
//使用时,slot-scop获取插槽对象的引用,items.data获取到自定义属性
<myComponent>
       <div slot-scop="items">{{items.data}}</div>
</myComponent>

//官方写法使用v-slot,slotProps是子组件的数据名称
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>
</todo-list>
```

**父组件的东西会在父组件的作用域斌阿姨,子组件模板的所有东西会在子组件的作用域编译**

# 图片懒加载

[官网地址](https://github.com/hilongjw/vue-lazyload)

```js
//第一步
npm install vue-lazyload --save
//第二步: 全局注册main.js 文件
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload)
// 配置项
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  attempt: 1
})
//第三步: component写loading图片的样式（不是必须, 视情况而定）
img[lazy="loading"]{
  display:block;
  width:50px !important;
  height:50px !important;
  margin:0 auto;
  }
//第四步: component使用 ( :src--->v-lazy )
  <div class="lazyLoad">
    <ul id="container">
      <li v-for="img in arr">
        <img v-lazy="img.thumbnail_pic_s">
      </li>
    </ul>
  </div>
```

# 路径别名

[路径别名](https://blog.csdn.net/panchang199266/article/details/90145638)

`vue.config.js`

```js
const path = require("path"); //引入path模块
function resolve(dir) {
  return path.join(__dirname, dir); //path.join(__dirname)设置绝对路径
}

module.exports = {
  chainWebpack: config => {
    config.resolve.alias
      //set第一个参数：设置的别名，第二个参数：设置的路径
      .set("@", resolve("./src")) //根据目录的层级来决定，也可以自行设
      .set("assets", resolve("./src/assets"))
      .set("css", resolve("./src/assets/css"))
      .set("img", resolve("./src/assets/img"))
      .set("common", resolve("./src/common"))
      .set("components", resolve("./src/components"))
      .set("common", resolve("./src/components/common"))
      .set("content", resolve("./src/components/content"))
      .set("network", resolve("./src/network"))
      .set("router", resolve("./src/router"))
      .set("store", resolve("./src/store"))
      .set("views", resolve("./src/views"));
  }
};
```



# 组件中的data为什么是一个函数???

```js
Vue.component('myComponent',{
  template:<div>{{num}}</div>,
  data(){
   return {
     num: 0
   }
  },
  methods:{
    increment(){
      this.num++
    }
  }
})
//js中函数具体作用域,可以想象当cpn该组件复用时其data中的属性由于作用域的原因,每个实例的data不会共享,而method是对象,每个实例的方法可以共享
```

# vue响应式原理

```js
<template>
    <div>{{name+age}}</div>
<template>

//vue内部如何监听数据改变(Object.defineProperty监听对象属性改变)

let obj = { //这是将要被监听的对象
    name: 'xu',
    age: 20
}
Object.keys(obj).forEach(key=>{ //通过obj.keys取出该对象所有属性
    Object.defineProperty(obj,key,{
        set(newValue){
            //此处监听key的对应value的改变
            console.log(`set ${key}: `,newValue)
            //问题1:谁在使用这个key呢???
            //解: 通过对template解析,使用该属性的变量会触发get函数,get函数中记录下这些变量
        },
        get(){
        	console.log(`get ${key}`)
            //返回该属性对应的value
            return 
    	}
    })
})
obj.name = 'honshen' //触发defineProperty的set函数获取name属性会触发get函数
//vue如何通知界面刷新(发布订阅者模式)

 //发布者
    class Dep {
        //保存所有的订阅者
        subs;
        constructor(){
            this.subs= []
        }
        //添加订阅者
        addWatcher(watcher){
            this.subs.push(watcher)
        }
        //通知所有的订阅者更新
        notify(){
            this.subs.forEach(sub=>sub.updata())
        }
    }
    //订阅者
    class Watcher{
        name;age;
        constructor(name,age){
            this.name=name;
            this.age=age
        }
        updata(){
            console.log(this.name+': updata')
        }
    }
```

```html
<!--实例-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <input
      type="text"
      id="inputText"
      placeholder="input data and then click the button"
      style="width: 250px"
    />
    <button onclick="handleBtnClick('name')">changeObjName</button>
    <button onclick="handleBtnClick('age')">changeObjAge</button>
    <div
      id="showObj"
      style="font-size: x-large; color: cadetblue; margin-top: 50px"
    ></div>
  </body>
  <script>
    //发布者
    class Dep {
      //保存所有的订阅者
      subs;
      //该订阅者属于哪个key
      key;
      constructor(key) {
        this.subs = [];
        this.key = key;
      }
      //添加订阅者
      addWatcher(watcher) {
        //如果订阅者不存在数组则添加
        console.log(this.key + "subs: ", this.subs);
        this.subs.every((e) => {
          console.log(e, watcher);
          return e.domElement != watcher.domElement;
        }) && this.subs.push(watcher);
      }
      //通知所有的订阅者更新
      notify() {
        this.subs.forEach((sub) => sub.updata());
      }
    }
    //订阅者
    class Watcher {
      domElement = null;
      constructor(domElement) {
        this.domElement = domElement;
      }
      updata() {
        this.domElement.innerHTML = `Object.name:${obj.name}
    Object.age: ${obj.age}`;
      }
    }
    let obj = {
      name: "",
      age: "",
    };
    let store = new Map();
    let deps = new Map();
    Object.keys(obj).forEach((key) => {
      //为每一个属性添加发布者
      deps.set(key, new Dep(key));
      //通过obj.keys取出该对象所有属性
      Object.defineProperty(obj, key, {
        set(newValue) {
          //此处监听key的对应value的改变
          console.log(`set ${key}: `, newValue);
          store.set(key, newValue);
          //问题1:谁在使用这个key呢???
          //解: 通过对template解析,使用该属性的变量会触发get函数,get函数中记录下这些变量
          deps.get(key).notify();
        },
        get() {
          console.log(`get ${key}`);
          deps.get(key).addWatcher(new Watcher(divObj));
          //返回该属性对应的value
          return store.get(key);
        },
      });
    });
    //赋初始值
    (obj.name = "xu"), (obj.age = 18);
  </script>
  <script>
    var input = document.querySelector("#inputText");
    var inputValue = "";
    input.oninput = (e) => (inputValue = e.target.value);
    var handleBtnClick = (key) => (obj[key] = inputValue);
    var divObj = document.querySelector("#showObj");
    console.log(obj);
    divObj.innerHTML = `Object.name:${obj.name}
    Object.age: ${obj.age}`;
  </script>
</html>
```

# Vue-router

​	[官方文档](https://router.vuejs.org/)

## 安装

​	`npm install --save vue-router`

## 文件结构

```js
src 
    + router
		+ index.js
```

`serc/router/index.js`

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  routes
})

export default router
```

## 路由渲染

`router-link`更改hash,`router-view`显示路由视图

```js
//tag可以指定link渲染成那个标签
<router-link to="/" tag="button" replace active-class="类名">Home</router-link> 
<router-link to="/about">About</router-link>
<router-view/>
```

## hash和history

```js
const router = new VueRouter({
  routes,
  mode: "history"
})
```

## 动态路由匹配

```js
const User = {
  template: '<div>User</div>'
}
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
```

现在呢，像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由。

一个“路径参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。于是，我们可以更新 `User` 的模板，输出当前用户的 ID：

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

你可以看看这个[在线例子 (opens new window)](https://jsfiddle.net/yyx990803/4xfa2f19/)。

你可以在一个路由中设置多段“路径参数”，对应的值都会设置到 `$route.params` 中。例如：

| 模式                          | 匹配路径            | $route.params                          |
| ----------------------------- | ------------------- | -------------------------------------- |
| /user/:username               | /user/evan          | `{ username: 'evan' }`                 |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |

除了 `$route.params` 外，`$route` 对象还提供了其它有用的信息，例如，`$route.query` (如果 URL 中有查询参数)、`$route.hash` 等等。你可以查看 [API 文档](https://router.vuejs.org/zh/api/#路由对象) 的详细说明。

## 路由传参

### 动态路由

```js
//路由
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
 //组件
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

### 查询

```js
//父组件
<router-link v-bind:to="{path:'/about',query:{
	变量:值
}}">About</router-link>
//路由组件
$route.query //获取传递到路由组件的参数
```

  ### 编程式传参

```js
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

## 响应路由参数的变化

提醒一下，当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。

复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) `$route` 对象：

```js
const User = {
  template: '...',
  watch: {
    $route(to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

## 捕获路由

匹配**任意路径**，我们可以使用通配符 (`*`)：

```js
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
{
    //匹配默认路径也可以 path: '/'
    path: ''
}
```

也就是说含有*通配符*的路由应该放在最后。路由 `{ path: '*' }` 通常用于客户端 404 错误。如果你使用了*History 模式*，请确保[正确配置你的服务器](https://router.vuejs.org/zh/guide/essentials/history-mode.html)。

当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分：

```js
// 给出一个路由 { path: '/user-*' }
this.$router.push('/user-admin')
this.$route.params.pathMatch // 'admin'
// 给出一个路由 { path: '*' }
this.$router.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'
```

## 嵌套路由

要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 `children` 配置：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
            //这个地方不能加/必须是单词
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

```js
//在user导航组件中
<router-link to="/user/me/profile" >profile</router-link>
//在userx组件中需要显示的地方
<router-view/>
```



**要注意，以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**你会发现，`children` 配置就是像 `routes` 配置一样的路由配置数组，所以呢，你可以嵌套多层路由。

此时，基于上面的配置，当你访问 `/user/foo` 时，`User` 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由

## 编程式的导航

```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

**注意：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path`：**

```js
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

## `$router`和`$router`

```js
$route是当前活跃的路由

```



##  命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象：

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

这跟代码调用 `router.push()` 是一回事：

## 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

## 重定向和别名

### 重定向

重定向也是通过 `routes` 配置来完成，下面例子是从 `/a` 重定向到 `/b`：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

重定向的目标也可以是一个命名的路由：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

甚至是一个方法，动态返回重定向目标：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

注意[导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)并没有应用在跳转路由上，而仅仅应用在其目标上。在下面这个例子中，为 `/a` 路由添加一个 `beforeEnter` 守卫并不会有任何效果。

### 别名

“重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`，那么“别名”又是什么呢？

**`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

上面对应的路由配置为：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。

##  路由组件传参

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦：

**取代与 `$route` 的耦合**

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [{ path: '/user/:id', component: User }]
})
```

**通过 `props` 解耦**

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

这样你便可以在任何地方使用该组件，使得该组件更易于重用和测试。

### [#](https://router.vuejs.org/zh/guide/essentials/passing-props.html#布尔模式)布尔模式

如果 `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。

### [#](https://router.vuejs.org/zh/guide/essentials/passing-props.html#对象模式)对象模式

如果 `props` 是一个对象，它会被按原样设置为组件属性。当 `props` 是静态的时候有用。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/promotion/from-newsletter',
      component: Promotion,
      props: { newsletterPopup: false }
    }
  ]
})
```

### [#](https://router.vuejs.org/zh/guide/essentials/passing-props.html#函数模式)函数模式

你可以创建一个函数返回 `props`。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/search',
      component: SearchUser,
      props: route => ({ query: route.query.q })
    }
  ]
})
```

URL `/search?q=vue` 会将 `{query: 'vue'}` 作为属性传递给 `SearchUser` 组件。

请尽可能保持 `props` 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 `props`，请使用包装组件，这样 Vue 才可以对状态变化做出反应。

## 导航守卫

### 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // 必须自己调用一下next
    next();
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

**确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

```js
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#全局解析守卫)全局解析守卫

> 2.5.0 新增

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#全局后置钩子)全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

```js
router.afterEach((to, from) => {
  // ...
})
```

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#路由独享的守卫)路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

这些守卫与全局前置守卫的方法参数是一样的。

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#组件内的守卫)组件内的守卫

最后，你可以在路由组件内直接定义以下路由导航守卫：

- `beforeRouteEnter`
- `beforeRouteUpdate` (2.2 新增)
- `beforeRouteLeave`

```js
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#完整的导航解析流程)完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

## 路由元信息

定义路由的时候可以配置 `meta` 字段：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

下面例子展示在全局导航守卫中检查元字段：

```js
router.beforeEach((to, from, next) => {
    //此处可以使用元数据嵌套时使用matched,不嵌套可以使用meta
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

## 过渡动效

[观看 Vue School 的如何创建路由过渡动效的免费视频课程 (英文)](https://vueschool.io/lessons/how-to-create-route-transitions-with-vue-router?friend=vuerouter)

`<router-view>` 是基本的动态组件，所以我们可以用 `<transition>` 组件给它添加一些过渡效果：

```html
<transition>
  <router-view></router-view>
</transition>
```

[Transition 的所有功能 (opens new window)](https://cn.vuejs.org/guide/transitions.html)在这里同样适用。

### [#](https://router.vuejs.org/zh/guide/advanced/transitions.html#单个路由的过渡)单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 `<transition>` 并设置不同的 name。

```js
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

### [#](https://router.vuejs.org/zh/guide/advanced/transitions.html#基于路由的动态过渡)基于路由的动态过渡

还可以基于当前路由与目标路由的变化关系，动态设置过渡效果：

```html
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

## 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错 —— 就看你想要的用户体验是哪种。

### [#](https://router.vuejs.org/zh/guide/advanced/data-fetching.html#导航完成后获取数据)导航完成后获取数据

当你使用这种方式时，我们会马上导航和渲染组件，然后在组件的 `created` 钩子中获取数据。这让我们有机会在数据获取期间展示一个 loading 状态，还可以在不同视图间展示不同的 loading 状态。

假设我们有一个 `Post` 组件，需要基于 `$route.params.id` 获取文章数据：

```html
<template>
  <div class="post">
    <div v-if="loading" class="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.fetchData()
  },
  watch: {
    // 如果路由有变化，会再次执行该方法
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      // replace getPost with your data fetching util / API wrapper
      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

### [#](https://router.vuejs.org/zh/guide/advanced/data-fetching.html#在导航完成前获取数据)在导航完成前获取数据

通过这种方式，我们在导航转入新的路由前获取数据。我们可以在接下来的组件的 `beforeRouteEnter` 守卫中获取数据，当数据获取成功后只调用 `next` 方法。

```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```

在为后面的视图获取数据时，用户会停留在当前的界面，因此建议在数据获取期间，显示一些进度条或者别的指示。如果数据获取失败，同样有必要展示一些全局的错误提醒。

## 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里 (opens new window)](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

举例：

```js
scrollBehavior (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

对于所有路由导航，简单地让页面滚动到顶部。

返回 `savedPosition`，在按下 后退/前进 按钮时，就会像浏览器的原生表现那样：

```js
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```

如果你要模拟“滚动到锚点”的行为：

```js
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash
    }
  }
}
```

我们还可以利用[路由元信息](https://router.vuejs.org/zh/guide/advanced/meta.html)更细颗粒度地控制滚动。查看完整例子请[移步这里 (opens new window)](https://github.com/vuejs/vue-router/blob/dev/examples/scroll-behavior/app.js)。

### [#](https://router.vuejs.org/zh/guide/advanced/scroll-behavior.html#异步滚动)异步滚动

> 2.8.0 新增

你也可以返回一个 Promise 来得出预期的位置描述：

```js
scrollBehavior (to, from, savedPosition) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ x: 0, y: 0 })
    }, 500)
  })
}
```

将其挂载到从页面级别的过渡组件的事件上，令其滚动行为和页面过渡一起良好运行是可能的。但是考虑到用例的多样性和复杂性，我们仅提供这个原始的接口，以支持不同用户场景的具体实现。

### [#](https://router.vuejs.org/zh/guide/advanced/scroll-behavior.html#平滑滚动)平滑滚动

只需将 `behavior` 选项添加到 `scrollBehavior` 内部返回的对象中，就可以为[支持它的浏览器 (opens new window)](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions/behavior)启用原生平滑滚动：

```js
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash,
      behavior: 'smooth',
    }
  }
}
```

## 路由懒加载

[观看 Vue School 的如何路由懒加载的免费视频课程 (英文)](https://vueschool.io/lessons/how-to-lazy-load-routes-with-vue-router?friend=vuerouter)

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件 (opens new window)](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能 (opens new window)](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```js
const Foo = () =>
  Promise.resolve({
    /* 组件定义对象 */
  })
```

第二，在 Webpack 2 中，我们可以使用[动态 import (opens new window)](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)：

```js
import('./Foo.vue') // 返回 Promise
```

注意

如果您使用的是 Babel，你将需要添加 [`syntax-dynamic-import` (opens new window)](https://babeljs.io/docs/plugins/syntax-dynamic-import/)插件，才能使 Babel 可以正确地解析语法。

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```js
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：

```js
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
})
```

### [#](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html#把组件按组分块)把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk (opens new window)](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

## 导航故障

> 3.4.0中新增

译者注

*导航故障*，或者叫*导航失败*，表示一次失败的导航，原文叫 navigation failures，本文统一采用*导航故障*。

当使用 `router-link` 组件时，Vue Router 会自动调用 `router.push` 来触发一次导航。 虽然大多数链接的预期行为是将用户导航到一个新页面，但也有少数情况下用户将留在同一页面上：

- 用户已经位于他们正在尝试导航到的页面
- 一个[导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)通过调用 `next(false)` 中断了这次导航
- 一个[导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)抛出了一个错误，或者调用了 `next(new Error())`

当使用 `router-link` 组件时，**这些失败都不会打印出错误**。然而，如果你使用 `router.push` 或者 `router.replace` 的话，可能会在控制台看到一条 *"Uncaught (in promise) Error"* 这样的错误，后面跟着一条更具体的消息。让我们来了解一下如何区分*导航故障*。

背景故事

在 v3.2.0 中，可以通过使用 `router.push` 的两个可选的回调函数：`onComplete` 和 `onAbort` 来暴露*导航故障*。从版本 3.1.0 开始，`router.push` 和 `router.replace` 在没有提供 `onComplete`/`onAbort` 回调的情况下会返回一个 *Promise*。这个 *Promise* 的 resolve 和 reject 将分别用来代替 `onComplete` 和 `onAbort` 的调用。

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html#检测导航故障)检测导航故障

*导航故障*是一个 `Error` 实例，附带了一些额外的属性。要检查一个错误是否来自于路由器，可以使用 `isNavigationFailure` 函数：

```js
import VueRouter from 'vue-router'
const { isNavigationFailure, NavigationFailureType } = VueRouter

// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    // 向用户显示一个小通知
    showToast('Login in order to access the admin panel')
  }
})
```

提示

如果你忽略第二个参数：`isNavigationFailure(failure)`，那么就只会检查这个错误是不是一个*导航故障*。

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html#navigationfailuretype)`NavigationFailureType`

`NavigationFailureType` 可以帮助开发者来区分不同类型的*导航故障*。有四种不同的类型：

- `redirected`：在导航守卫中调用了 `next(newLocation)` 重定向到了其他地方。
- `aborted`：在导航守卫中调用了 `next(false)` 中断了本次导航。
- `cancelled`：在当前导航还没有完成之前又有了一个新的导航。比如，在等待导航守卫的过程中又调用了 `router.push`。
- `duplicated`：导航被阻止，因为我们已经在目标位置了。

### [#](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html#导航故障的属性)*导航故障*的属性

所有的导航故障都会有 `to` 和 `from` 属性，分别用来表达这次失败的导航的目标位置和当前位置。

```js
// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    failure.to.path // '/admin'
    failure.from.path // '/'
  }
})
```

在所有情况下，`to` 和 `from` 都是规范化的路由位置。

## keep-alive

`将router-view`包裹在组件`keep-alive`中,组件视图会被缓存

此时会多两个钩子`actived`和`deactived`,分别是当组件处于活跃和不活跃时调用

属性:`include`字符串或正则表达式只有匹配的组件会被缓存

​	`exclude`字符串或正则表达式只有匹配的组件不会缓存

```js
<keep-alive exclude="index,about"><router-view /></keep-alive>
```

# Vuex

​	[官网](https://vuex.vuejs.org/)

## State

#### [#](https://vuex.vuejs.org/zh/guide/state.html#单一状态树)单一状态树

Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 ([SSOT (opens new window)](https://en.wikipedia.org/wiki/Single_source_of_truth))”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

单状态树和模块化并不冲突——在后面的章节里我们会讨论如何将状态和状态变更事件分布到各个子模块中。

存储在 Vuex 中的数据和 Vue 实例中的 `data` 遵循相同的规则，例如状态对象必须是纯粹 (plain) 的。**参考：**[Vue#data (opens new window)](https://cn.vuejs.org/v2/api/#data)。

**因此只有一个stroe就行了**

### watcher

`state`中的属性只要赋予了初始值后,其中改变都会被watcher观察到并响应

```js
export default new Vuex.Store({
  state: {
      stu:{
         name: 'xu',
          age: 18
      }
  },
    mutations: {
     changename(state){
         state.stu.name = 'honshen'
         //以下视图不会更新因为没有被初始化
         state.stu.me='我'
         //通过Vue,set更新
     }
  },
})
//加入到state的对象其中的属性变化都会被响应与react不同
//数组的方法大都是响应式但arr[0]=1不会是响应式
//手动调用视图更新
//Vue.set(数组,索引,值)
//Vue.set(对象,属性,值) vue会将新的属性添加到响应式系统中
//Vue.delete(对象,属性) 删除属性
```

#### [#](https://vuex.vuejs.org/zh/guide/state.html#在-vue-组件中获得-vuex-状态)在 Vue 组件中获得 Vuex 状态

那么我们如何在 Vue 组件中展示状态呢？由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在[计算属性 (opens new window)](https://cn.vuejs.org/guide/computed.html)中返回某个状态：

```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

每当 `store.state.count` 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

然而，这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态。

Vuex 通过 `store` 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需调用 `Vue.use(Vuex)`）：

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。让我们更新下 `Counter` 的实现：

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
        //通过$store获取store
      return this.$store.state.count
    }
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/state.html#mapstate-辅助函数)`mapState` 辅助函数

[在 Scrimba 上尝试这节课](https://scrimba.com/p/pnyzgAP/c8Pz7BSK)

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

#### [#](https://vuex.vuejs.org/zh/guide/state.html#对象展开运算符)对象展开运算符

`mapState` 函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 `computed` 属性。但是自从有了[对象展开运算符 (opens new window)](https://github.com/tc39/proposal-object-rest-spread)，我们可以极大地简化写法：

```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

#### [#](https://vuex.vuejs.org/zh/guide/state.html#组件仍然保有局部状态)组件仍然保有局部状态

使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

## Getter

有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 接受 state 作为其第一个参数：

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

#### [#](https://vuex.vuejs.org/zh/guide/getters.html#通过属性访问)通过属性访问

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值：

```js
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
store.getters.doneTodosCount // -> 1
```

我们可以很容易地在任何组件中使用它：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

注意，getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。

#### [#](https://vuex.vuejs.org/zh/guide/getters.html#通过方法访问)通过方法访问geeter传参

你也可以通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组进行查询时非常有用。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

注意，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。

#### [#](https://vuex.vuejs.org/zh/guide/getters.html#mapgetters-辅助函数)`mapGetters` 辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

如果你想将一个 getter 属性另取一个名字，使用对象形式：

```js
...mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```

## Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

让我们来注册一个简单的 action：

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。当我们在之后介绍到 [Modules](https://vuex.vuejs.org/zh/guide/modules.html) 时，你就知道 context 对象为什么不是 store 实例本身了。

实践中，我们会经常用到 ES2015 的 [参数解构 (opens new window)](https://github.com/lukehoban/es6features#destructuring)来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/actions.html#分发-action)分发 Action

Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('increment')
```

乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？实际上并非如此，还记得 **mutation 必须同步执行**这个限制么？Action 就不受约束！我们可以在 action 内部执行**异步**操作：

```js
actions: {
    //默认参数是context 上下文是一个对象,playload是传递参数
  incrementAsync ({ commit },palyload) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

Actions 支持同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

来看一个更加实际的购物车示例，涉及到**调用异步 API** 和**分发多重 mutation**：

```js
actions: {
  checkout ({ commit, state }, products) {
    // 把当前购物车的物品备份起来
    const savedCartItems = [...state.cart.added]
    // 发出结账请求，然后乐观地清空购物车
    commit(types.CHECKOUT_REQUEST)
    // 购物 API 接受一个成功回调和一个失败回调
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失败操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

注意我们正在进行一系列的异步操作，并且通过提交 mutation 来记录 action 产生的副作用（即状态变更）。

#### [#](https://vuex.vuejs.org/zh/guide/actions.html#在组件中分发-action)在组件中分发 Action

你在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用（需要先在根节点注入 `store`）：

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/actions.html#组合-action)组合 Action

Action 通常是异步的，那么如何知道 action 什么时候结束呢？更重要的是，我们如何才能组合多个 action，以处理更加复杂的异步流程？

首先，你需要明白 `store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise：

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

现在你可以：

```js
 store.dispatch('actionA').then(() => {
  // ...
})
```

在另外一个 action 中也可以：

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

最后，如果我们利用 [async / await (opens new window)](https://tc39.github.io/ecmascript-asyncawait/)，我们可以如下组合 action：

```js
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

> 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。

## Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
      //此处参数会将state传递
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法：

```js
store.commit('increment')
```

### [#](https://vuex.vuejs.org/zh/guide/mutations.html#提交载荷-payload)提交载荷（Payload）

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**：

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})
```

#### [#](https://vuex.vuejs.org/zh/guide/mutations.html#对象风格的提交方式)对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

### [#](https://vuex.vuejs.org/zh/guide/mutations.html#mutation-需遵守-vue-的响应规则)Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`, 或者

- 以新对象替换老对象。例如，利用[对象展开运算符 (opens new window)](https://github.com/tc39/proposal-object-rest-spread)我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

### [#](https://vuex.vuejs.org/zh/guide/mutations.html#使用常量替代-mutation-事件类型)使用常量替代 Mutation 事件类型

使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

用不用常量取决于你——在需要多人协作的大型项目中，这会很有帮助。但如果你不喜欢，你完全可以不这样做。

### [#](https://vuex.vuejs.org/zh/guide/mutations.html#mutation-必须是同步函数)Mutation 必须是同步函数

一条重要的原则就是要记住 **mutation 必须是同步函数**。为什么？请参考下面的例子：

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

#### [#](https://vuex.vuejs.org/zh/guide/mutations.html#在组件中提交-mutation)在组件中提交 Mutation

你可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation，或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/mutations.html#下一步-action)Action

在 mutation 中混合异步调用会导致你的程序很难调试。例如，当你调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？这就是为什么我们要区分这两个概念。在 Vuex 中，**mutation 都是同步事务**：

```js
store.commit('increment')
// 任何由 "increment" 导致的状态变更都应该在此刻完成。
```

## Module

[在 Scrimba 上尝试这节课](https://scrimba.com/p/pnyzgAP/cqKK4psq)

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

//组件使用状态
$store.state.a // -> moduleA 的状态
$store.state.b // -> moduleB 的状态
//组件提交
this.$store.commit('') //mutations不要和模块mutations里的方法重名
```

### [#](https://vuex.vuejs.org/zh/guide/modules.html#模块的局部状态)模块的局部状态

对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  // ...
  actions: {
      //rootState可以访问store里的state,此处的commit只有提交模块内的方法
      //默认参数是context有很多东西可以拿到rootGetter
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

### [#](https://vuex.vuejs.org/zh/guide/modules.html#命名空间)命名空间

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。例如：

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 进一步嵌套命名空间
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容（module assets）时不需要在同一模块内额外添加空间名前缀。更改 `namespaced` 属性后不需要修改模块内的代码。

#### [#](https://vuex.vuejs.org/zh/guide/modules.html#在带命名空间的模块内访问全局内容-global-assets)在带命名空间的模块内访问全局内容（Global Assets）

如果你希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为第三和第四参数传入 getter，也会通过 `context` 对象的属性传入 action。

若需要在全局命名空间内分发 action 或提交 mutation，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit` 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/modules.html#在带命名空间的模块注册全局-action)在带命名空间的模块注册全局 action

若需要在带命名空间的模块注册全局 action，你可添加 `root: true`，并将这个 action 的定义放在函数 `handler` 中。例如：

```js
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,

      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/modules.html#带命名空间的绑定函数)带命名空间的绑定函数

当使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐：

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

对于这种情况，你可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。于是上面的例子可以简化为：

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```

而且，你可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数：

```js
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```

#### [#](https://vuex.vuejs.org/zh/guide/modules.html#给插件开发者的注意事项)给插件开发者的注意事项

如果你开发的[插件（Plugin）](https://vuex.vuejs.org/zh/guide/plugins.html)提供了模块并允许用户将其添加到 Vuex store，可能需要考虑模块的空间名称问题。对于这种情况，你可以通过插件的参数对象来允许用户指定空间名称：

```js
// 通过插件的参数对象得到空间名称
// 然后返回 Vuex 插件函数
export function createPlugin (options = {}) {
  return function (store) {
    // 把空间名字添加到插件模块的类型（type）中去
    const namespace = options.namespace || ''
    store.dispatch(namespace + 'pluginAction')
  }
}
```

### [#](https://vuex.vuejs.org/zh/guide/modules.html#模块动态注册)模块动态注册

在 store 创建**之后**，你可以使用 `store.registerModule` 方法注册模块：

```js
import Vuex from 'vuex'

const store = new Vuex.Store({ /* 选项 */ })

// 注册模块 `myModule`
store.registerModule('myModule', {
  // ...
})
// 注册嵌套模块 `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
```

之后就可以通过 `store.state.myModule` 和 `store.state.nested.myModule` 访问模块的状态。

模块动态注册功能使得其他 Vue 插件可以通过在 store 中附加新模块的方式来使用 Vuex 管理状态。例如，[`vuex-router-sync` (opens new window)](https://github.com/vuejs/vuex-router-sync)插件就是通过动态注册模块将 vue-router 和 vuex 结合在一起，实现应用的路由状态管理。

你也可以使用 `store.unregisterModule(moduleName)` 来动态卸载模块。注意，你不能使用此方法卸载静态模块（即创建 store 时声明的模块）。

注意，你可以通过 `store.hasModule(moduleName)` 方法检查该模块是否已经被注册到 store。

#### [#](https://vuex.vuejs.org/zh/guide/modules.html#保留-state)保留 state

在注册一个新 module 时，你很有可能想保留过去的 state，例如从一个服务端渲染的应用保留 state。你可以通过 `preserveState` 选项将其归档：`store.registerModule('a', module, { preserveState: true })`。

当你设置 `preserveState: true` 时，该模块会被注册，action、mutation 和 getter 会被添加到 store 中，但是 state 不会。这里假设 store 的 state 已经包含了这个 module 的 state 并且你不希望将其覆写。

### [#](https://vuex.vuejs.org/zh/guide/modules.html#模块重用)模块重用

有时我们可能需要创建一个模块的多个实例，例如：

- 创建多个 store，他们公用同一个模块 (例如当 `runInNewContext` 选项是 `false` 或 `'once'` 时，为了[在服务端渲染中避免有状态的单例 (opens new window)](https://ssr.vuejs.org/en/structure.html#avoid-stateful-singletons))
- 在一个 store 中多次注册同一个模块

如果我们使用一个纯对象来声明模块的状态，那么这个状态对象会通过引用被共享，导致状态对象被修改时 store 或模块间数据互相污染的问题。

实际上这和 Vue 组件内的 `data` 是同样的问题。因此解决办法也是相同的——使用一个函数来声明模块状态（仅 2.3.0+ 支持）：

```js
const MyReusableModule = {
  state: () => ({
    foo: 'bar'
  }),
  // mutation, action 和 getter 等等...
}
```

## 项目结构

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

只要你遵守以上规则，如何组织代码随你便。如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

## 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`：

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

### [#](https://vuex.vuejs.org/zh/guide/strict.html#开发环境与发布环境)开发环境与发布环境

**不要在发布环境下启用严格模式**！严格模式会深度监测状态树来检测不合规的状态变更——请确保在发布环境下关闭严格模式，以避免性能损失。

类似于插件，我们可以让构建工具来处理这种情况：

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```

# 第三方库

**第三库都应该被封装一下!!!如果哪天第三方库不在更新和维护这样便于修改**

## Better-scroll

​	[丝滑滚动英文](https://better-scroll.github.io/docs/en-US/guide/)

​	[丝滑滚动中文](https://better-scroll.github.io/docs/zh-CN/guide/#betterscroll-%E6%98%AF%E4%BB%80%E4%B9%88)

​	[github](https://github.com/ustbhuangyi/better-scroll)

- **pullingDown(\**pull-down\**)**

  - **触发时机**：当顶部下拉距离超过阈值

  ```js
    import BetterScroll from '@better-scroll/core'
    import Pulldown from '@better-scroll/pull-down'
  
    BetterScroll.use(Pulldown)
    const bs = new BetterScroll('.wrapper', {
      pullDownRefresh: true
    })
  
    bs.on('pullingDown', () => {
      await fetchData()
        //注意这个函数必须要调用
      bs.finishPullDown()
    })
  ```

- **pullingUp(\**pull-up\**)**

  - **触发时机**：当底部下拉距离超过阈值

  ```js
    import BetterScroll from '@better-scroll/core'
    import Pullup from '@better-scroll/pull-up'
  
    BetterScroll.use(Pullup)
    const bs = new BetterScroll('.wrapper', {
      pullUpLoad: true
    })
  
    bs.on('pullingUp', () => {
      await fetchData()
        //这个函数必须要调用,该函数可以实现节流防止多次刷新
      bs.finishPullUp()
    })
  ```

```js
/*上拉加载更多导致页面不滚动解决方案*/
export default{
    props:['probeType','pullUpLoad']
    data(){return{scroll:null}}
    mounted(){
        //created触发时页面元素未挂载获取不到
        this.scroll = new BetterScroll(this.$refs.warpper,{
            //允许滚动视图内点击事件
    		click: true,
            //派发滚动事件,该值建议为属性由引用该组件的父组件决定
            probeType:this.probeType
            //派发上拉加载事件
            pullUpLoad: this.pullUpLoad
            //解决异步加载图片卡顿不能滚动
            observeDom:true,
            observeImage:true
		})
        //监听滚动回调
        this.scroll.on('scroll',(position)=>{console.log(position)})
         // 3.监听上拉事件
      	this.scroll.on('pullingUp', () => {this.$emit('pullingUp') })
    }
}
// 1. probeType 为 0，在任何时候都不派发 scroll 事件，
// 2. probeType 为 1，仅仅当手指按在滚动区域上，每隔 momentumLimitTime 毫秒派发一次 scroll 事件，
// 3. probeType 为 2，仅仅当手指按在滚动区域上，一直派发 scroll 事件，
// 4. probeType 为 3，任何时候都派发 scroll 事件，包括调用 scrollTo 或者触发 momentum 滚动动画
/*
refresh()
参数：无
返回值：无
作用：重新计算 BetterScroll，当 DOM 结构发生变化的时候务必要调用确保滚动的效果正常。
*/
```

## fastClick

​	[https://github.com/ftlabs/fastclick](https://github.com/ftlabs/fastclick)

## 安装

`npm install --save fastclick`

## vue使用

```js
//main.js
import fastClick from 'fastclick

fastClick.attach(document.body)
```

## css单位转化--移动端常用

[配置1 to rem](https://blog.csdn.net/weixin_46995731/article/details/108429963)

[配置2 to viewport](https://www.cnblogs.com/zhangnan35/p/12682925.html)
