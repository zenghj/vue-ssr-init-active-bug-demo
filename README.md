# vue ssr客户端激活 (client-side hydration)时与服务端渲染结果不一致的bug demo

服务端渲染的结果需要和客户端初次激活时dom结构一致，否则，在开发模式下会用客户端渲染内容替代服务端渲染内容，并在控制台报错:
>vendors.app.js:214 error [Vue warn]: The client-side rendered virtual DOM tree is not matching server-rendered content. This is likely caused by incorrect HTML markup, for example nesting block-level elements inside , or missing . Bailing hydration and performing full client-side render.

而在生产环境下，（以单个dom元素为例，服务端渲染和客户端激活时元素tag类型不同）则是Dom元素类型保持为服务端渲染的结果，元素属性会被客户端渲染结果更新（相同属性由客户端渲染结果更新，仅服务端渲染结果有的属性或者仅客户端渲染结果有的属性保留）。
例如
```html
    <div>
      <span class="client-render" v-if="isClient || isMounted" @click="handleClick" data-test="client">xx client</span>     
      <a class="server-render" v-else href="https://m.baidu.com/index.php" data-test="server">xx server</a>
    </div>
```
服务端渲染结果是
```html
<div>
  <a class="server-render" href="https://m.baidu.com/index.php" data-test="server">xx server</a>
</div>
```
客户端渲染结果是
```html
<div>
  <span class="client-render"  @click="handleClick" data-test="client">xx client</span>
</div>
```

客户端激活之后最终结果会是
```html
<div>
  <a href="https://m.baidu.com/index.php" @click="handleClick" data-test="client" class="client-render">xx client</a>
</div>
```