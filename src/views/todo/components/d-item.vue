<template>
  <li class="todo-item" :class="{'completed': currentItem.completed}" @dblclick="dblClickHandler" ref="item">
    <div class="todo-item_label" v-show="!isEdit" >
      <input class="check" type="checkbox" v-model="currentItem.completed"/>
      <label class="check-icon"></label>
      <label class="text">{{currentItem.value}}</label>
      <button class="close" @click="closeHandler">X</button>
    </div>
    <input class="d-item_content" v-model="currentItem.value" ref="input" @keydown.enter="enterHandler" v-if="isEdit"/>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      default: () => ({})
    },
    index: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      currentItem: {},
      isEdit: false
    }
  },
  watch: {
    item: {
      handler (val) {
        this.currentItem = val
      },
      immediate: true
    },
    currentItem (val) {
      this.$emit('input', val)
    }
  },
  methods: {
    // 双击
    dblClickHandler () {
      console.log('2342342')
      this.isEdit = true
      // 无法获取input
      // console.log(this.$refs.input)
      // this.$refs.input.focus()

      this.$nextTick(() => {
        console.log(this.$refs.input, this.$refs.item)
        this.$refs.input.focus()
      })
    },
    // 删除按钮
    closeHandler () {
      this.$emit('on-delete', this.currentItem)
    },
    enterHandler () {
      this.isEdit = false
    }
  }
}
</script>

<style lang="less" scoped>
input{
  outline: none;
  font-size: 24px;
}
li{
  list-style:none
}
.todo-item{
  display: flex;
  height: 65px;
  position: relative;
  &.completed{
    .text{
      color: #d9d9d9;
      text-decoration: line-through;
    }
    .check-icon{
      &::before{
      content: '√';
      font-size: 16px;
      color: green;
      line-height: 30px;
      text-align: center;
    }
    }
  }
  .todo-item_label{
    width: 100%;
    &:hover{
      .close{
        display: block;
        outline: none;
      }
    }
    .check{
      opacity: 0;  // 隐藏原有图标
      display: block;
      height:  40px;
      width: 40px;
      margin: auto 0;
      position: absolute;
      top: 0;
      bottom: 0;
      z-index: 2;
      // 第一种控制选中样式
      // &:checked{
      //   & + .check-icon{
      //     &::before{
      //       content: '√';
      //       font-size: 16px;
      //       color: green;
      //       line-height: 30px;
      //       text-align: center;
      //     }
      //   }
      // }
    }
    .check-icon{
      display: block;
      height:  30px;
      width: 30px;
      margin: auto 0;
      position: absolute;
      left: 10px;
      top: 0;
      bottom: 0;
      border: 1px solid #ccc;
      border-radius: 50%;
    }
    .text{
      padding: 15px 15px 15px 60px;
      width: 100%;
      height: 100%;
      display: block;
      box-sizing: border-box;
      text-align: left;
      line-height: 35px;
    }
    .close{
      border: none;
      display: none;
      position: absolute;
      top: 0;
      right: 10px;
      bottom: 0;
      width: 40px;
      height: 40px;
      margin: auto 0;
      font-size: 18px;
      color: #cc9a9a;
      transition: color 0.2s ease-out;
      // transform: translateY(-50%);
    }
  }
  .d-item_content{
    flex-grow: 1;
    padding: 16px 16px 16px 0;
    border: none;
    background: rgba(0, 0, 0, 0.003);
    line-height: 33px;
    width: 100%;
    outline: none;
    font-size: 24px;
    font-weight: 300;
    margin-left: 60px;
  }
}
</style>
