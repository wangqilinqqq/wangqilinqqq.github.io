---
title: vue创建项目
date: 2017-03-30 15:16:46
categories: Vue
tags: Vue
---

## 安装

需要安装node 和 vue-cli 脚手架，node的安装方法多种多样，直接 [进入官网](https://nodejs.org/en/ "node官网") 下载

一. 安装全局vue-cli
>npm install vue-cli -g

ps:检查是否安装成功可用: vue -V 有显示版本号就是安装成功了
<!--more-->


二. 创建项目
>vue init webpack vue2-demo


![Alt text](/images/vue-study/code1.png)

三. 进入到vue2-demo，安装依赖

>npm install


四.安装webpack
>npm install webpack --save-dev

 **-dev** 指的是在生产环节中使用，如果需要在线上环境中使用，就不需要带-dev


五.本地运行

>npm run dev

浏览器输出 `http://localhost:8080` 即可访问
