<template>
  <button class="btn" :class="{'btn-default': !active, 'btn-active': active}" @click="clickHandler">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'd-button',
  props: {
    type: {
      type: String,
      default: 'default'
    },
    active: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {}
  },
  methods: {
    clickHandler () {
      this.$emit('click')
    }
  }
}
</script>

<style lang="less" scoped>
 .btn{
  outline: none; // 去掉点击按钮时的样式
  border: none;
  padding: 6px;
  margin-left: 5px;
  position: relative;
  z-index: 10;
  &.btn-active{
    color: red;
    border: 1px solid #FF7F50;
  }
 }
 .btn-default:hover{
    padding: 5px;
    border: 1px solid #FF8C00;
  }

</style>
