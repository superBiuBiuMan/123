## 前置

* 配合在线演示看更好~
* [@在线演示地址](https://codesandbox.io/s/condescending-frost-rn4u7b?file=/src/components/HelloWorld.vue)

## 引入的方式

### 相对路径的引用

#### webpack的处理

* 路径以`.`开头,会被 webpack 处理
* 在其编译过程中，所有诸如
* `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL **都会被解析为一个模块依赖**。

例如,`url(./image.png)`会被编译为`require("./image.png")`而:

```html
<img src="./image.png">
```

将会被编译到：

```js
h('img', { attrs: { src: require('./image.png') }})
```

* 并且,会根据图片的大小来决定是否转化为base64编码图片

#### 相对路径下的URL转换规则

- 如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于你的文件系统中的目录结构进行解析。
  - 大白话就是哪里引入的,就相当与哪一个文件的路径
- `<img src="./img/28.jpg" alt="" />`就表示引入当前组件下`img`目录下的`28.jpg`文件

示例如下

**src/components/HelloWorld.vue**

```html
<template>
  <div class="hello">
    <!-- 相对路径的引入静态资源 -->
    <img src="./img/28.jpg" alt="" />
  </div>
</template>
```

引入的就是`src/components/img/28.jpg`,参照于HelloWord.vue所在的路径,也就是以`src/components`为起始点,去寻找`img`目录下的`28.jpg`文件

![](https://dreamos.oss-cn-beijing.aliyuncs.com/gitblog/202208211536272.png)

### 绝对路径的引用

* **需要知道的是**在vue当中,绝对路径的表示是以URL为`/`开头的,或是省略了`/`或者是`.`
  * 比如`/images/foo.png`就表示是一个绝对路径在vue当中
  * `images/foo.png`也表示一个绝对路径在vue当中

#### webpack的处理

* 依旧会根据图片的大小来决定是否转化为base64编码图片

![](https://dreamos.oss-cn-beijing.aliyuncs.com/gitblog/202208211554186.png)

#### 绝对路径下的URL转换规则

* 绝对路径的表示是以URL为`/`开头的,或是省略了`/`或者是`.`

  * 比如`/images/foo.png`就表示是一个绝对路径在vue当中

  * `images/foo.png`也表示一个绝对路径在vue当中

* **转换规则很简单**,绝对路径的转换规则就是以`public`为起始点

  * `<img src="/resource/img/pic1.png" alt="" />`
  * 就表示引入`public/img/pic1.png`文件

示例代码如下

```html
<template>
  <div class="hello">
    <!-- 相对路径的引入静态资源 -->
    <img src="./img/28.jpg" alt="" title="相对路径的引入静态资源" />
    <!-- url路径含有@ -->
    <img src="@/components/img/28.jpg" alt="" title="url路径含有@" />
    <!-- 绝对路径的引用 -->
    <img src="/resource/img/pic1.png" alt="" />
    <img src="resource/img/pic1.png" alt="" />
  </div>
</template>
```

* 在绝对路径的引用当中
  * `src="/resource/img/pic1.png"`和`src="resource/img/pic1.png"`
  * 引入的都是`public/img/pic1.png`文件

![](https://dreamos.oss-cn-beijing.aliyuncs.com/gitblog/202208211556235.png)

### 含有'@'路径URL的转换规则

* 如有URL以`@`开头,它也会作为一个模块请求被解析,它的用户在于Vue Cli默认会设置一个执行`<projectRoot>/src`的别名`@`(**仅作用于模板中**)
  * **大白话说**:就是看到路径有`@`了,就说明是`项目路径/src`路径为起始点了

**src/components/HelloWorld.vue**

```html
<template>
  <div class="hello">
    <!-- 相对路径的引入静态资源 -->
    <img src="./img/28.jpg" alt="" />
    <!-- url路径含有@ 二个写法都是一样的-->
    <img src="@/components/img/28.jpg" alt="" />
  </div>
</template>
```

* 上面代码的示例当中
  * `src = "./img/28.jpg"`和`src="@/components/img/28.jpg"`都是指向`src/components/img/28.jpg`的图片文件

![](https://dreamos.oss-cn-beijing.aliyuncs.com/gitblog/202208211543452.png)

* 并且图片也都可以这样子引入
  * `<img :src="require('@/assets/46.jpg')" alt="">`

### 含有'~'路径URL的转换规则(主要使用好像是css里面)

* 含有`~`路径URL多用于css当中
* vue的解释
  * 如果 URL 以 ~ 开头会作为一个模块请求被解析从`node_modules`中引用资源。
  * 反正我是看不太懂vue的解释是什么意思....所以这里就记录下使用情况,多在css中使用

css里面,如果使用**相对路径**去引入图片,那没有什么区别,都会引入`src/assets/26.jpg`文件

```css
<style>
.show-bg {
  /* 使用相对路径引入图片在css中 */
  background-image: url("../assets/26.jpg");
}
</style>
```

如果使用了**绝对路径**去引入图片,那么不添加`~`,就会报错

```css
<style>
.show-bg {
  /* 使用相对路径引入图片在css中 */
  /* background-image: url("../assets/26.jpg"); */
  /* 使用绝对路径引入图片在css中 */
  background-image: url("@/assets/26.jpg");

}
</style>
```

![报错信息](https://dreamos.oss-cn-beijing.aliyuncs.com/gitblog/202208211718962.png)

所以需要添加下`~`就可以

```css
<style>
.show-bg {
  /* 使用相对路径引入图片在css中 */
  /* background-image: url("../assets/26.jpg"); */
  /* 使用绝对路径引入图片在css中 */
  background-image: url("~@/assets/26.jpg");

}
</style>
```

### data当中图片数据的引入

* 一开始我以为直接在data数据当中写图片路径就可以,然后循环,后面发现不可以

```html
<template>
  <div>
    <div class="show-pic">
      <!-- :src="picPath" 还是require(picPath) 都不可以 -->
      <img
        v-for="(picPath, index) in imgList"
        :key="index"
        :src="picPath"
        alt="123"
      />
    </div>
  </div>
</template>
```

```js
  data() {
    return {
      name: "李白",
      imgList: [
        "./assets/46.jpg", 
        "./assets/47.jpg", 
        "./assets/48.jpg"],
    };
  },
```

* **后面发现需要先在data当中引入才可以**

```html
<template>
  <div>
    <div class="show-pic">
      <img
        v-for="(picPath, index) in imgList"
        :key="index"
        :src="picPath"
        alt="123"
      />
    </div>
  </div>
</template>
```

```js
  data() {
    return {
      name: "李白",
      imgList: [
        require("./assets/46.jpg"), 
        require("./assets/47.jpg"), 
        require("./assets/48.jpg")],
    };
  },
```

### 总结

vue引入静态资源有**相对路径**和**绝对路径**的方式

**相对路径**是URL开头有`.`

**绝对路径**是URL开头为`/`或者省略不写

**相对路径**相对的是当前文件

**绝对路径**相对的是`public`目录

并且引入的时候URL开头为`~`的使用在css里面引入静态资源的时候

如果 URL 以 @ 开头会作为一个模块请求被解析。Vue CLI 默认会设置一个指向 src 的别名 @ 。

### 参考文章

> https://segmentfault.com/a/1190000021485662
>
> https://segmentfault.com/a/1190000019495695

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
