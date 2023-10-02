#wip #frontend #案例梳理 
# 场景

在`casdoor-vue-sdk`的`1.4.0`版本更新后，加入了`async function`的`api`，从而使得`babel`需要转译，而这依赖于`_asyncToGenerator`。比较奇怪的是，其打包后`_asyncToGenerator`的引入路径竟然是从根目录开始的。在查询了将近两个小时的文档后，终于有了一些头绪。

## 找到根源

在从`babel.config.js`去除`@vue/cli-plugin-babel/preset`之后，引入路径就正常了，更甚者，当我进一步把`.babelrc`中的`@babel/plugin-transform-runtime`去掉，`_asyncToGenerator`函数甚至直接打包进了产物里。这个结果令人惊喜，毕竟这基本是我要的结果，但是这个过程中发生了什么？

返璞归真，我将所有的`babel`插件去除，只留下`@vue/cli-plugin-babel/preset`的配置时，其又出现了从根目录开始引用`_asyncToGenerator`的引入路径，最终全部去除，终于还原了对`async function`不动手脚的`babel`配置。

那么，究竟`@vue/cli-plugin-babel/preset`做了什么事情呢？

# babel的基本原理与`plugin`和`preset`的作用

`babel`的基本原理是将`js`转译成`AST`（抽象语法树），通过`plugin`对语法树进行修改，以实现从`es6`转译到`es5`，以及类似的能力。而`preset`实际上就是一组预先设置好的`plugin`与配置，以便利的对特定某种开发环境一次性完成`plugin`的设置。

## `@vue/cli-plugin-babel/preset`做了什么

定位到问题之后，我们可以从[仓库](https://github.com/vuejs/vue-cli/blob/dev/packages/%40vue/babel-preset-app/index.js)中找到以下段落:

```js
// transform runtime, but only for helpers

plugins.push([require('@babel/plugin-transform-runtime'), {

regenerator: useBuiltIns !== 'usage',

// polyfills are injected by preset-env & polyfillsPlugin, so no need to add them again

corejs: false,

helpers: useBuiltIns === 'usage',

useESModules: !process.env.VUE_CLI_BABEL_TRANSPILE_MODULES,

absoluteRuntime,

version

}])
```

其中`absolute runtime`配置为字符串，值如下:
```js
const runtimePath = path.dirname(require.resolve('@babel/runtime/package.json'))
```

根据这个配置，`helper`的引入都会来自`@babel/runtime/package.json`所在的绝对路径，很明显对于需要打包`npm`插件给别人的`casdoor-vue-sdk`，这样的产物是不合理的。

## 解决方案

显然的解决思路有两个:

1. 去掉`@vue/cli-plugin-babel/preset`并重新配置
2. 修改`@vue/cli-plugin-babel/preset`中`plugin-transform-runtime`的配置

实际上通过上面的仓库文档，我们可以发现其并没有提供这方面的配置项，而`babel`本身也不提供我们暴力覆写`preset`的功能，因此只有方案一是可行的。

## 方案一

首先是，`@vue/cli-plugin-babel/preset`是做什么的呢？可以看到其仓库文档开头便是

	This is the default Babel preset used in all Vue CLI projects. **Note: this preset is meant to be used exclusively in projects created via Vue CLI and does not consider external use cases.**

这边的`external use cases`可能指的是用在`Vue CLI`以外创建的项目，但也很可能是指我们现在的情况，也就是打包项目给外部使用。其次显然的，`Vue CLI`通常用在创建`SPA`而非`Vue Plugin`，和我们的场景也有很大区别，去掉是必须的。

而我们项目中目前需要的也仅有`module-resolver`帮我们在`babel`转义时将`path alias`转成相对路径，以及将`es6`语法`polyfill`到`es5`以支持较旧版本的浏览器。


