<template>
  <div>
    <car-keyboard
      :show="show"
      :value="value"
      @select="onShow"
      @close="onHide"
    />
    <p @click="onHide">关闭键盘</p>
  </div>
</template>

<script>
import Keyboard from "./lib";

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
      this.show = true
    },
    onHide() {
      this.show = false
    }
  }
}
</script>

<style>

</style>
