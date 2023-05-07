---
title: VUE基本语法
tags:
  - VUE
categories:
  - Third-week
---

# Vue基本语法

vue语法的笔记我只记了所学过的内容和应用场景

vue基本模版

```js
			<script>
        //createApp 表示创建一个Vue应用,存储到app中
        //传入的参数表示这个应用最外层的组件
        //mvvm设计模式,m -> model 数据, v -> View 视图 , vm -> viewModel 视图数据连接
        const app = Vue.createApp({
            data(){
                return {
                    inputValue: '',
                    list: []
                }
            },
            methods: {
                handleAddItem(){
                    this.list.push(this.inputValue);
                    this.inputValue = ''
                }
            },
            template: `
              <div>
                <input v-model="inputValue" />
                <button v-on:click = "handleAddItem">Add {{inputValue}}</button>
                <ul>
                  <li v-for="(item, index) of list">{{item}}  {{index}}</li>
                </ul>
              </div>
            
            `
        })
        //vm表示Vue应用的根节点
        const vm = app.mount('#root')
        </script>
```

```js
v-if  v-show
v-else-if
v-else

v-for="{item, index/key, [index]} in list"

事件修饰符:stop,prevent, capture, self, once, passive
按键修饰符:enter,tab,delete,esc,up,down,left,right
鼠标修饰符:left,right,middle
精确修饰符:exact

双向绑定 v-model
```

> computed：computed 是一种用于声明计算属性的方式

watch：用于监听数据的变化然后执行回调函数，每监听到一次数据改变就会触发一次回调函数，回调函数的两个参数分别是newValue，oldValue
> 

```js
组件component

组件传参数(父传子)
参数校验: validator 设置逻辑判断,返回boolean
					 type   限制属性
				 default  设置默认值

全局/局部组件

单项数据流的概念:数据从父组件流向子组件

inheritAttrs :是否继承根组件的属性.如果为false,那么在子组件中需要通过this.$attr.attrName
来访问父组件传递给子组件的属性

slot 插槽,具名插槽
keep-alive:保持动态组件的状态
异步组件

v-once 让某个元素标签只渲染一次
ref获取DOm节点/组件引用
provide/inject:provide 和 inject 通常成对一起使用，使一个祖先组件作为其后代组件的依赖注入方，无论这个组件的层级有多深都可以注入成功，只要他们处于同一条组件链上。(vue官方文档)
```

> 子传父 
在子组件中用this.emit(”funName”,”message”)  把子组件的讯息传给父组件

另一种方法是用model-value实现父子组件间的数据双向绑定
> 

```js
<div id="root"></div>
    <script>
        const app = Vue.createApp({
            data(){
                return{message:''}
            },
            methods(){

            },
            template:`
                {{message}}
                <test v-model="message" />
            `
        })

        app.component("test",{
            props:{
                modelValue:{
                    type:String,
                    required: true
                }
            },
            methods:{
                updateValue(event){
                    this.$emit('update:modelValue', event.target.value)
                }
            },
            template:`
            <input :value="modelValue" / @input="updateValue">
            `
        })

        app.mount("#root")
    </script>
```