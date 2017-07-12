---
title: vue-router 学习笔记
date: 2017-07-12 15:16:46
categories: Vue
tags: Vue
---

## 第一步：引入与配置 

在vue中使用路由的第一步总是引入路由

```bash
import Vue from 'vue'
import vueRouter from 'vue-router' //引入路由模块
import routes from './router/router' //引入路由配置文件


Vue.use(vueRouter) //vue 使用插件

const router = new VueRouter({//为路由配置路线
	routes
})

const app = new Vue({
	router
}).$mount("#app")
```

<!--more-->

## 第二步：编辑路由配置文件

新建router目录，在里面新建router.js

``` bash
//router.js
//引入组件-默认的第一个组件，一定是App.vue
import App from '../App' 
import Home from '../pages/home'
import About from '../pages/about'


//组件的引入还可以是按需引入，即需要哪个组件的时候才开始加载那个组件
//说明：webpack 在编译时，会静态地解析代码中的 require.ensure()，同时将模块添加到一个分开的 chunk 当中。这个新的 chunk 会被 webpack 通过 jsonp 来按需加载。
//参数说明：第一个参数是组件的路径，第二个是名称。webpack，会将名称相同的组件一起打包，加载的时候一起加载
const Home = r => require.ensure([], () => r(require('../pages/home')), 'home')
const About = r => require.ensure([], () => r(require('../pages/about')), 'about')


//配置路由
export default [{
	path: "/",
	component: App,
	children: [{
		path: '/',
		component: Home,  //如果有嵌套路由的话 在配置一个children,同事在home.vue中需要写上 <router-view></router-view>
		children:[{
			//...
		}]
	},{
		path: '/home',
		component: Home
	},{
		path: '/about',
		component: About
	}]
}]

```

