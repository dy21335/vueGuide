## 前期准备
 - 安装node.js,webstorm的基本设置（要在webstorm的editt - configurations里添加npm和node）；

## vue-cli的一些解释
 - 参考网站
https://my.oschina.net/liuyuantao/blog/883251
https://github.com/nodejh/vue2-tutorials

使用vue-cli初始化一个基于webpack模板的新项目后，项目目录为：
 > |-- build                            // 项目构建(webpack)相关代码    
|   |-- build.js                     // 生产环境构建代码  
|   |-- check-version.js             // 检查node、npm等版本  
|   |-- dev-client.js                // 热重载相关  
|   |-- dev-server.js                // 构建本地服务器  
|   |-- utils.js                     // 构建工具相关  
|   |-- webpack.base.conf.js         // webpack基础配置  
|   |-- webpack.dev.conf.js          // webpack开发环境配置  
|   |-- webpack.prod.conf.js         // webpack生产环境配置  
|-- config                           // 项目开发环境配置  
|   |-- dev.env.js                   // 开发环境变量  
|   |-- index.js                     // 项目一些配置变量  
|   |-- prod.env.js                  // 生产环境变量  
|   |-- test.env.js                  // 测试环境变量  
|-- src                              // 源码目录      
|   |-- components                     // vue公共组件    
|   |-- store                          // vuex的状态管理    
|   |-- App.vue                        // 页面入口文件  
|   |-- main.js                        // 程序入口文件，加载各种公共组件  
|-- static                           // 静态文件，比如一些图片，json数据等  
|   |-- data                           // 群聊分析得到的数据用于数据可视化  
|-- .babelrc                         // ES6语法编译配置  
|-- .editorconfig                    // 定义代码格式  
|-- .gitignore                       // git上传需要忽略的文件格式  
|-- README.md                        // 项目说明  
|-- favicon.ico   
|-- index.html                       // 入口页面  
|-- package.json                     // 项目基本信息  


代码的后缀名为.vue：
`.vue` Vue 组件，里面定义了 Vue 实例、模板、样式等。需要由 webpack 等工具来转换为 js 代码。

## package.json
`package.json`文件是项目根目录下的一个文件，定义该项目开发所需要的各种模块以及一些项目配置信息（如项目名称、版本、描述、作者等）。

`package.json`里的`scripts`字段，这个字段定义了你可以用npm运行的命令。在开发环境下，在命令行工具中运行`npm run dev `就相当于执行` node build/dev-server.js ` .也就是开启了一个node写的开发行建议服务器。由此可以看出script字段是用来指定npm相关命令的缩写。


## main.js
`main.js` 是项目的入口文件，也是 webpack 打包的入口文件。

```js
new Vue({
  el: '#app',
  template: '<App/>',
  components: { App },
});
```


#### components
`components: { App }` 等价于 `components: { App: App }`。只有在 `components` 里面列出来的组件，才可以在 `template` 里面使用。

如果我们把 `components: { App }`改为 `components: { MyApp: App }`，那么在 `template` 里面就需要这样使用：`template: '<my-app />'`。

由于 HTML 标签不区分大小写，所以 `components` 里面的驼峰命名会自动转换为短横线。详见 [camelCase vs. kebab-case](https://cn.vuejs.org/v2/guide/components.html#camelCase-vs-kebab-case)


#### template

`template` 就是挂载到页面的模板。
也就是说:`template: '<App/>' `表示用`<app></app>`替换`index.html`里面的`<div id="app"></div>`。
这里的值是 `<App/>` 组件就是 `components` 属性中的 `App`，也就是通过 `import` 引入的 `App` 这个模板。


打开App.vue


## App.vue

```js
<template>
<div id="app">
<img src="./assets/logo.png">
<router-view/>
</div>
</template>

<script>
export default {
name: 'App'
}
</script>

<style>
#app {
font-family: 'Avenir', Helvetica, Arial, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-align: center;
color: #2c3e50;
margin-top: 60px;
}
</style>
```

app.vue文件我们可以分成三部分解读:
>  <template></template>标签包裹的内容：这是模板的HTMLDom结构，里边引入了一张图片和<router-view></router-view>标签，<router-view>标签说明使用了路由机制。

> <script></script>标签包括的js内容：你可以在这里些一些页面的动态效果和Vue的逻辑代码。

> <style></style>标签包裹的css内容：这里就是你平时写的CSS样式，对页面样子进行装饰用的，需要特别说明的是你可以用<style scoped></style>来声明这些css样式只在本模板中起作用。


可见app.vue里引用了`<router-view/>`组件

## router/index.js路由文件
```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)

export default new Router({
routes: [
{
path: '/',
name: 'HelloWorld',
component: HelloWorld
}
]
})
```
我们可以看到 `import Hello from ‘@/components/Hello’`这句话， 文件引入了`/components/Hello.vue`文件。这个文件里就配置了一个路由，就是当我们访问网站时给我们显示`Hello.vue`的内容。
