<template>
  <div>
    <!-- 显示区域 -->
    <div class="__car-keyboard-view">
      <section
        v-for="(_, index) in 8"
        :key="index"
        class="__car-keyboard-word"
        :class="{
          '__car-keyboard-word-active': active === index,
          '__car-keyboard-word-2': index === 2
        }"
        @click="onClickWord(index)"
      >{{ getValue(index) }}
      </section>
    </div>

    <div v-show="show" class="__car-keyboard-wrap">
      <section class="__car-keyboard-close" @click="close">{{ closeText }}</section>
      <!-- 省份 -->
      <div v-show="active === 0" class="__car-keyboard-province">
        <section v-for="(row, rowIndex) in provinceList" :key="rowIndex" class="__car-keyboard-row">
          <section
            v-for="(cell, cellIndex) in row"
            :key="`${rowIndex}${cellIndex}`"
            class="__car-keyboard-cell"
            @click="onClickKeyboard('province', rowIndex, cellIndex)"
          >{{ cell }}
          </section>
        </section>
      </div>
      <!-- 字母 -->
      <div v-show="active > 0" class="__car-keyboard-container">
        <section v-for="(row, rowIndex) in keyboardList" :key="rowIndex" class="__car-keyboard-row">
          <section
            v-for="(cell, cellIndex) in row"
            :key="`${rowIndex}${cellIndex}`"
            class="__car-keyboard-cell"
            @click="onClickKeyboard('keyboard', rowIndex, cellIndex)"
          >{{ cell }}
          </section>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'car-keyboard',
  props: {
    value: {
      type: Array,
      required: true,
    },
    show: {
      type: Boolean,
      required: true,
    },
    closeText: {
      type: String,
      default: '关闭',
    },
  },
  data() {
    return {
      provinceList: Object.freeze([
        ['京', '沪', '粤', '津', '冀', '晋', '蒙', '辽'],
        ['吉', '黑', '苏', '浙', '皖', '闽', '赣', '鲁'],
        ['豫', '鄂', '湘', '桂', '琼', '渝', '川', '贵'],
        ['云', '藏', '陕', '甘', '青', '宁', '新', '使'],
      ]),
      keyboardList: Object.freeze([
        ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'],
        ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
        ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
        ['Z', 'X', 'C', 'V', 'B', 'N', 'M'],
        ['港', '澳', '学', '警', '领', '删除']
      ]),
      active: -1,
    }
  },
  watch: {
    show(value) {
      // 关闭时，清除光标
      if (!value) {
        this.active = -1
      }
    }
  },
  methods: {
    // 获取显示内容，主要是第8位新能源的判断
    getValue(index) {
      const { value } = this
      const word = value[index]
      let res = ''
      if (typeof word === 'string' && word !== '') { // 有值，直接返回
        res = word;
      } else if (index === 7 && this.active !== 7) { // 当前第8位且不高亮，返回+
        res = '+';
      }
      return res
    },
    // 点击车牌内容区域
    onClickWord(index) {
      this.active = index
      this.$emit('select')
    },
    // 关闭
    close() {
      this.$emit('close')
    },
    // 点击键盘区域
    onClickKeyboard(type, rowIndex, cellIndex) {
      // 获取键盘值
      let word = this[`${type}List`][rowIndex][cellIndex]
      const { active, value } = this
      // 替换输入值
      if (word !== '删除') { // 正常按键
        value[active] = word
        const word_7 = value[7]
        // 光标在前7位 或者 (光标在第7位 并且第8位为空时)，光标后移
        if (active < 6 || (active === 6 && typeof word_7 === 'string' && word_7 !== '')) {
          this.active++
        } else { // 否则关闭
          this.close()
        }
      } else { // 删除按键
        value[active] = undefined
        this.active--
      }
    }
  }
}
</script>

<style lang="less">
// 显示区域
.__car-keyboard-view {
  display: flex;

  .__car-keyboard-word {
    border: 1px solid #eee;
    background: #FFF;
    width: 30px;
    height: 44px;
    line-height: 44px;
    text-align: center;
    // 高亮
    &.__car-keyboard-word-active {
      border: 1px solid #02A7F0;
    }

    &.__car-keyboard-word-2 {
      margin-left: 20px;
    }
  }
}

.__car-keyboard-wrap {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  box-shadow: 0 0 6px 0 #ccc;
  // 关闭
  .__car-keyboard-close {
    text-align: center;
    line-height: 40px;
  }

  // 键盘
  .__car-keyboard-province, .__car-keyboard-container {
    background: #eceef1;
    padding: 12px 6px;

    .__car-keyboard-row {
      display: flex;
      justify-content: center;
      margin-top: 12px;

      &:first-child {
        margin-top: 0;
      }

      .__car-keyboard-cell {
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0 4px;
        width: 36px;
        flex-grow: 1;
        height: 36px;
        color: #333;
        background: #FFF;
        border-radius: 8px;
        box-shadow: 0 2px 0 #aaa;

        &:active {
          background: #b3b7bf;
        }
      }
    }
  }
}
</style>
