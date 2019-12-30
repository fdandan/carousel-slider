<template>
  <div class='carousel-wrap' :style="{height: height + 'px'}">
    <transition-group tag="ul" class='slide-ul' :name="disX === 0 ? 'list-left' : disX > 0 ? 'list-right' : 'list-left'" ref="slide">
        <li v-for="(item, index) in images" :key="index + item"
          v-show="index === currentIndex"
          @mouseenter="stop"
          @mouseleave="go"
          @click="next"
          @touchstart="touchStart"
          @touchmove="touchMove"
          @touchend="touchEnd">
          <img :src="item" alt="图片介绍">
        </li>
      </transition-group>
      <div class="carousel-items" v-show="showIndex">
        <span>{{currentIndex + 1}}/{{images.length}}</span>
      </div>
    </div>
</template>

<script>

export default {
  name: 'slider',
  props: {
    autoPlay: {
      type: Boolean,
      default: true
    },
    duration: {
      type: Number,
      default: 3000
    },
    images: {
      type: Array,
      default: () => []
    },
    showIndex: {
      type: Boolean,
      default: true
    },
    height: {
      type: Number,
      default: 347
    }
  },
  data () {
    return {
      timer: null,
      currentIndex: 0,
      startX: 0,
      moveX: 0,
      endX: 0,
      disX: 0
    }
  },
  computed: {},
  components: {},
  watch: {},
  methods: {
    doAutoPlay () {
      this.currentIndex++
      if (this.currentIndex > this.images.length - 1) {
        this.currentIndex = 0
      }
    },
    go () {
      if (this.autoPlay) {
        this.timer = setInterval(() => {
          this.doAutoPlay()
        }, this.duration)
      }
    },
    next () {
      this.doAutoPlay()
    },
    stop () {
      this.timer && clearInterval(this.timer)
      this.timer = null
    },
    touchStart (e) {
      this.startX = e.touches[0].clientX
      // 移动端上触发了touch就不需要在触发click了
      e.preventDefault()
    },
    touchMove (e) {
      this.moveX = e.touches[0].clientX
    },
    touchEnd (e) {
      this.endX = e.changedTouches[0].clientX
      this.disX = this.endX - this.startX
      if (this.disX > 0) {
        this.currentIndex--
      } else {
        this.currentIndex++
      }
      if (this.currentIndex > this.images.length - 1) {
        this.currentIndex = 0
      } else if (this.currentIndex < 0) {
        this.currentIndex = this.images.length - 1
      }
      console.log(this.disX)
    }
  },
  created () {
    this.$nextTick(() => {
      if (this.autoPlay) {
        this.timer = setInterval(() => {
          this.doAutoPlay()
        }, this.duration)
      }
    })
  },
  mounted () {}
}
</script>

<style scoped>
.carousel-wrap {
  width: 100%;
  overflow: hidden;
  position: relative;
}
.slide-ul {
  position: relative;
  width: 100%;
  height: 100%;
  list-style: none;
  padding: 0;
  margin: 0;
  cursor: pointer;
}
.slide-ul li {
  position: absolute;
  width: 100%;
  height: 100%;
}
img {
  width: 100%;
}
.list-left-enter-active,
.list-left-leave-active,
.list-right-enter-active,
.list-right-leave-active {
  transition: all 1s cubic-bezier(0.19, 1, 0.22, 1);
}

.list-left-enter,
.list-right-leave-to {
  transform: translateX(100%);
}

.list-right-enter,
.list-left-leave-to {
  transform: translateX(-100%);
}
.carousel-items {
  position: absolute;
  z-index: 10;
  right: 15px;
  bottom: 12px;
  width: 100%;
}

.carousel-items span {
  height: 19px;
  line-height: 19px;
  width: 48px;
  text-align: center;
  color: #fff;
  background-color: rgba(0,0,0,.3);
  border-radius: 12px;
  position: absolute;
  right: 15px;
  bottom: 12px;
  font-size: 12px;
}
.carousel-items .active {
  background-color: orange;
}
</style>
