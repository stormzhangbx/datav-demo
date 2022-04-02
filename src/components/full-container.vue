<template>
  <div class="full-screen" ref="fullScreen">
    <template v-if="ready">
      <slot></slot>
    </template>
  </div>
</template>

<script>
import { debounce } from '../util/index'

export default {
  name: "full-container",
  data() {
    return {
      ready: false,
      // 屏幕宽、高
      allWidth: 0,
      allHeight: 0,
      // 当前浏览器可用区域宽高
      currentWidth: 0,
      currentHeight: 0,
      debounceInitWHFun: null
    }
  },
  mounted() {
    this.allWidth = screen.width
    this.allHeight = screen.height

    this.$refs.fullScreen.style.width = `${screen.width}px`
    this.$refs.fullScreen.style.height = `${screen.height}px`

    this.debounceInitWHFun = debounce(100, this.resizeFn)
    this.debounceInitWHFun()
    window.addEventListener('resize', this.debounceInitWHFun)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.debounceInitWHFun)
  },
  methods: {
    resizeFn() {
      this.currentWidth = document.body.clientWidth
      this.currentHeight = document.body.clientHeight
      this.$refs.fullScreen.style.transform =
          `scale(${this.currentWidth / this.allWidth}, ${this.currentHeight / this.allHeight})`
      this.ready = true
    },
  }
}
</script>

<style scoped>
.full-screen {
  position: fixed;
  top: 0;
  left: 0;
  overflow: hidden;
  transform-origin: left top;
  z-index: 999;
}
</style>
