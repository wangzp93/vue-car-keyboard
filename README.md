# vue-car-keyboard

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 使用
```shell
npm install vue-car-keyboard -S
```

```html
<template>
  <div>
    <Keyboard
      :show="show"
      :value="value"
      @select="onShow"
      @close="onHide"
    />
    <p @click="onHide">关闭键盘</p>
  </div>
</template>
```

```javascript
import Keyboard from "vue-car-keyboard";

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
```

### 属性
| 名称 | 类型 | 默认值 | 描述 |
| :---- | ------ | ------ | ------ |
| show | Boolean | 必须 | 键盘显示开关 |
| value | Array | 必须 | 键盘值，数组形式 |
| closeText | String | '关闭' | 关闭键盘的按钮文案 |

### 事件
| 名称 | 参数 | 描述 |
| :---- | ------ | ------ |
| select |  | 点击输入区域的事件，一般在这里开启键盘 |
| close |  | 点击关闭键盘的事件，一般在这里关闭键盘 |
