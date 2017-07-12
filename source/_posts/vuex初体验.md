---
title: Vuex初体验
date: 2017-03-30 15:16:46
categories: Vuex
tags: Vuex
---
## 概述

&nbsp;&nbsp;&nbsp;&nbsp;vue将一个项目分成N多个组件，组件与组件之间相结合，就构成了一个完整的项目，但是当组件足够多的时候，组件与组件之前的通讯将会变得异常复杂、但是用Vuex就会完全不一样、下面这个例子是一个非常简单的例子，在组件A中改变值，组件B中相对应的更新值。

## 新建项目
使用[Vue-cli新建项目](https://wangqilinqqq.github.io/2017/03/30/vue创建项目/ 'title')

## 安装 vuex

>npm install vuex --save


## 开始
我们的根组件是 App.vue 他下面有2个子组件 button.vue 和 info.vue

**这个是button.vue**
``` bash
//button.vue

<template>
	<div>
		<button>count +1</button>
	</div>
</template>

<script>
	export default {
	}
</script>
```

**这个是info.vue**
``` bash
//info.vue

<template>
	<div>
		<p>count is 0</p>
	</div>
</template>

<script>

	export default {
	}
</script>

```

**然后在我们的根组件中引入子组件**
``` bash
//App.vue

<template>
	<div id="app">
		<my-info></my-info>
		<my-botton></my-botton>
	</div>
</template>

<script>
	import MyInfo from './components/info.vue';
	import MyButton from './components/button.vue';

	export default {
		components: {
			'my-info': MyInfo,
			'my-botton': MyButton
		}
	}
</script>

```

启动应用

>npm run dev

浏览器输入 http://localhost:8080/
![Alt text](/images/vuex/pic1.png)
好的 基本已经完成，非常简单的布局，我们现在要做的就是 点击按钮一次，上面的数字也同步更新


## Vuex-Store


**进入我们的目录，在src文件夹，创建一个文件夹名为 store,并新建 index文件**


``` bash

//index.js

/*vue模块，最开始引用*/import Vue from 'vue';
/*引入vuex模块*/import Vuex from 'vuex';

/*应用Vuex*/Vue.use(Vuex)



const state = {
	//这里存储着所有需要共享的状态
	count: 0
}

const mutations = {
	//这里是操作状态改变,注意不能直接调用此方法，
	//需要使用 store.commin('increment')->参数与这里定义的函数名相同
	increment: state => state.count++
}

export default new Vuex.Store({
	state,
	mutations
})
```
**接着创建getters.js文件**

``` bash
//此模块主要用于获取状态

export default {
	getCount () {
		return this.$store.state.count
	}
}

```

**然后在创建actions.js文件**

``` bash
//此文件主要用于改变状态

export default {
	increment: function() {
		this.$store.commit('increment')
	}
}

```

**在  main.js 文件中 将 store 绑定在 Vue 实例上，任何地方的组件都可以通过 this.$store 来访问这个对象**

``` bash
import Vue from 'vue'
import store from './store/' /*引入文件默认是index 所以是缩写了*/
import App from './App'

const app = new Vue({
	el: '#app',
	store, /*在这里绑定了之后任何组件都可以通过 this.$store 来访问里面的属性与方法*/
	render: h => h(App)
})
```

**回到info.vue组件在里面使用count**

``` bash
<template>
	<div>
		<p>count is {{count}}</p>
	</div>
</template>

<script>
	import getters from '../store/getters'

	export default {
		computed: {
			count: getters.getCount
		}
	}
</script>
```

**回到button.vue组件中定义方法*

``` bash

<template>
	<div>
		<button @click='incrementCount'>count +1</button>
	</div>
</template>

<script>
	import actions from '../store/actions'

	export default {
		methods: {
			incrementCount: actions.increment
		}
	}
</script>

```

然后运行项目

>num run dev

  over~
