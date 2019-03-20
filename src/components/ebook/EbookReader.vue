<template>
  <div class="ebook-reader">
    <div id="read"></div>
  </div>
</template>

<script>
// import { mapActions } from 'vuex'
import { ebookMixin } from '../../utils/mixin'
import Epub from 'epubjs'
global.ePub = Epub
export default {
  mixins: [ebookMixin],
  methods: {
    // ...mapActions(['setMenuVisible']),
    prevPage () {
      if (this.rendition) {
        this.rendition.prev()
        this.hideTitleAndMenu()
      }
    },
    nextPage () {
      if (this.rendition) {
        this.rendition.next()
        this.hideTitleAndMenu()
      }
    },
    toggleTitleAndMenu () {
      // console.log('menuVisible')
      // this.$store.dispatch('setMenuVisible', !this.menuVisible)
      if (this.menuVisible) {
        this.setSettingVisible(-1)
      }
      this.setMenuVisible(!this.menuVisible)
    },
    hideTitleAndMenu () {
      // this.$store.dispatch('setMenuVisible', false)
      this.setMenuVisible(false)
      this.setSettingVisible(-1)
    },
    initEpub () {
      const url = 'http://192.168.1.5:8081/epub/' + this.fileName + '.epub'
      // console.log(url)
      this.book = new Epub(url)
      this.setCurrentBook(this.book)
      // console.log(this.book)
      this.rendition = this.book.renderTo('read', {
        width: innerWidth,
        height: innerHeight,
        method: 'default'
      })
      this.rendition.display()
      this.rendition.on('touchstart', event => {
        // console.log(event)
        this.touchStartX = event.changedTouches[0].clientX
        this.touchStartTime = event.timeStamp
      })
      this.rendition.on('touchend', event => {
        const offsetX = event.changedTouches[0].clientX - this.touchStartX
        const time = event.timeStamp - this.touchStartTime
        // console.log(offsetX, time)
        if (time < 500 && offsetX > 40) {
          this.prevPage()
        } else if (time < 500 && offsetX < -40) {
          this.nextPage()
        } else {
          this.toggleTitleAndMenu()
        }
        event.preventDefault()
        event.stopPropagation()
      })
    }
  },
  mounted () {
    // this.$store.dispatch('setFileName', this.$route.params.fileName.split('|').join('/'))
    this.setFileName(this.$route.params.fileName.split('|').join('/'))
    .then(() => {
      this.initEpub()
    })
  }
}
</script>
