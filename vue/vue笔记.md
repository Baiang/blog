##v-bind属性绑定
数据绑定一个常见需求是操作元素的 class 列表和它的内联样式。因为它们都是属性，我们可以用 v-bind 处理它们：我们只需要计算出表达式最终的字符串。不过，字符串拼接麻烦又易错。因此，在v-bind 用于 class 和 style 时，Vue.js 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组。


	//html代码
	<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>
	
	//js代码
	data: {
  		isA: true,
  		isB: false
	}

	渲染为
	<div class="static class-a"></div>

##组件开发
Vue是专门view数据渲染的MVVM框架，其中最核心的是组件开发。组件注册后便可以用在父实例的模块中，以自定义元素的形式使用要确保在初始化根实例之**前**注册了组件

###全局组件注册


	//html代码
	<my-component></my-component>

	//js代码
	Vue.component('my-component', {
        template: '<div>A custom component! <my-c1></my-c1></div>'
    })

###局部组件注册
局部注册组件，只能使用在父组件模板内

	//html代码
	<div id="Parent"></div>

	//js代码
	var Parent = Vue.extend({
        template: '<div>I\'m Parent, My children: <my-c1></my-c1></div>',
        components: {
              //只能用在父组件模板
              'my-c1':{
               		template: '<div>A custom component!</div>'
               }
         }
     })
	
	//组件实例化
     new Parent({
         	el: '#Parent'
     })

##组件通讯
组件实例的作用域是孤立的。这意味着不能并且不应该在子组件的模板内直接引用父组件的数据。

###props
“prop”是组件数据的一个字段，期望从父组件传下来

	//html代码
	<child msg="hello!"></child>

	//js代码
	 Vue.component('child', {
          // 声明 props
          props: ['msg'],
          // prop 可以用在模板内
          // 可以用 `this.msg` 设置
          template: '<span>{{ msg }}</span>'
      })
	
	//渲染为
	<span>hello!</span>

###动态 Props
与v-bind属性绑定结合使用

	//html代码
	<child :msg="hello!"></child>

	//js代码
	Vue.component('child', {
          // 声明 props
          props: ['msg'],
          // prop 可以用在模板内
          // 可以用 `this.msg` 设置
          template: '<span>{{ msg.a }}</span>'
        })
	new Vue({
	    el: '#app',
	    data:{
	        msgs: {
	            a: 1
	        }
	    }
	})

与v-mode结合v-bind结合一起使用
	
	//html代码
	<input v-model="parentMsg">
      <br>
    <child :message="parentMsg"></child>

	//js代码
	Vue.component('child', {
	  // 声明 props
	  props: ['message'],
	  // prop 可以用在模板内
	  // 可以用 `this.msg` 设置
	  template: '<span>{{ message }}</span>'
	})
	
	new Vue({
	    el: '#app',
	    data:{
	        parentMsg: 'Message from parent'
	    }
	})

![](http://i.imgur.com/p0iJuox.png)