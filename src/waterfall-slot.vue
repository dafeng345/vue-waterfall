<template>
  <div class="vue-waterfall-slot" v-show="isShow">
    <slot></slot>
  </div>
</template>

<style>
.vue-waterfall-slot {
  position: absolute;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<script>

export default {
  data: () => ({
    isShow: false
  }),
  props: {
    // 传值：父waterfall传子
    width: {
      required: true,
      validator: (val) => val >= 0
    },
    height: {
      required: true,
      validator: (val) => val >= 0
    },
    order: {
      default: 0
    },
    moveClass: {
      default: ''
    }
  },
  methods: {
    notify () {
      // this.$parent==waterfalll组件，在父组件中on监听  1-2
      this.$parent.$emit('reflow', this)
    },
    // 获得子组件的信息
    getMeta () {
      return {
        vm: this,
        node: this.$el,
        order: this.order,
        width: this.width,
        height: this.height,
        moveClass: this.moveClass
      }
    }
  },
  created () {
    this.rect = {
      top: 0,
      left: 0,
      width: 0,
      height: 0
    }
    this.$watch(() => (
      this.width,
      this.height
    ), this.notify)
  },
  mounted () {
    // 只监听一次事件
    this.$parent.$once('reflowed', () => {
      this.isShow = true
    })
    // 程序入口 1-1
    this.notify()
  },
  destroyed () {
    this.notify()
  }
}

</script>
