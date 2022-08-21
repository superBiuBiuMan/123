<template>
  <div class="hello">
    <h3>相对路径的引入静态资源</h3>
    <!-- 相对路径的引入静态资源 -->
    <img src="./img/28.jpg" alt="" title="相对路径的引入静态资源" />
    <hr />
    <h3>url路径开头为@</h3>
    <!-- url路径开头为@ -->
    <img src="@/components/img/28.jpg" alt="" title="url路径含有@" />
    <hr />
    <h3>绝对路径的引用</h3>
    <!-- 绝对路径的引用 -->
    <img src="/resource/img/pic1.png" alt="" />
    <img src="resource/img/pic1.png" alt="" />
    <hr />
    <h3>url路径开头为~的使用</h3>
    <!-- url路径开头为~的使用 -->
    <div class="show-pic">
      <!-- <img
        v-for="(picPath, index) in imgList"
        :key="index"
        :src="'~' + picPath"
        alt=""
      /> -->
    </div>
    <hr />
    <!-- css当中的引入 -->
    <h3>css当中的引入</h3>
    <div class="show-bg" style="width: 400px; height: 200px">
      <!-- 显示背景图 -->
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      imgList: ["./img/46.jpg", "./img/47.jpg", "./img/48.jpg"],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.show-bg {
  /* background-image: url("https://www.dreamlove.top/img/favicon.png"); */
  background-image: url("./img/28.jpg");
}
img {
  display: block;
  width: 100px;
  height: 100px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
