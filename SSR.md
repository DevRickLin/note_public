#wip #frontend #性能 
# SSR的优势与劣势

## 优势

1. FP及FCP优化(因为完整的html已经生成好了)
2. 更好的SEO

## 劣势

1. 在服务器上运算导致服务器性能负载， TTFB（首字节时间）变慢
2. CDN缓存不可用

## SSR进一步优化

1. 通过http缓存减少SSR运算
2. 在静态页面采用SSG更合适

### SSR with Hydration
1. 现在大部分SSR都采用了同构渲染技术，只在服务端进行首屏渲染，接着到客户端进行hydration（添加event handler)
	1. 对应有`ReactDom.hydrate`
	2. 服务端负载较小
	3. TTI(首次交互)性能较差

### Streaming SSR

-   較佳的 TTFB, FCP, TTI
-   Node.js Server 可以承受較多請求
-   支援 SEO

Streaming SSR 的缺點是有些使用情境沒辦法 streaming，例如：

-   某些 CSS framework 需要掃一遍頁面以產生 critical CSS
-   需要用 `renderToStaticMarkup()` 產生頁面內容

### Progressive Hydration

1.  支援 SSR
2.  以 component 為單位做 code splitting
3.  支援元件以自訂順序做 hydration
4.  不會在 hydration 過程中 block 使用者的互動
5.  在 hydration 過程中顯示 loading indicator
6. 可以基于React18 Concurrent Mode实现

https://shubo.io/rendering-patterns/#ssr-with-hydration

# Vue SSR 实现原理
1. 用createSSRApp创建实例
2. 通过renderToString在服务端生成返回html
3. 通过在客户端对createSSRApp实例调用mount方法
4. 是同构渲染？

## Vue SSR 友好
1. `onMounted`和`onUpdated`生命周期钩子不会被调用
2. 其他待补充

https://vuejs.org/guide/scaling-up/ssr.html