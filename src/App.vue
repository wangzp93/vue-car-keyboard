<template>
  <div>
    <Keyboard
      :show="show"
      :value="value"
      @select="onShow"
      @on-close="onHide"
    />
    <p @click="onHide">关闭键盘</p>
  </div>
</template>

<script>
import Keyboard from "./lib";

console.log(Keyboard)

export default {
  name: 'app',
  components: {
    [Keyboard.name]: Keyboard,
  },
  data () {
    return {
      value: [],
      show: false
    }
  },
  methods: {
    onShow() {
      console.log('show')
      this.show = true
    },
    onHide() {
      console.log('hide')
      this.show = false
    }
  }
}
</script>

<style>

</style>
