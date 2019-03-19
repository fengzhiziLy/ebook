<template>
  <div class="home">
    <!-- <div id="read"></div> -->
    <span class="icon-bookmark"></span>
    <span class="icon-clock"></span>
  </div>
</template>

<script>
import Epub from 'epubjs'
global.ePub = Epub

export default {
  // name: 'home',
  // mounted () {
  //   this.book = new Epub('/相约星期二 - [美] 米奇·阿尔博姆.epub')
  //   // console.log(this.book)
  //   this.book.renderTo('read', {
  //     width: window.innerWidth,
  //     height: window.innerHeight
  //   }).display()
  // }
}
</script>

<style scoped>
.icon-bookmark {
  font-size: 30px;
  color: red;
}
</style>
