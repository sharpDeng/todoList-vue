/* eslint-disable vue/return-in-computed-property */
<template>
  <div class="page">
    <!-- 第一步完成框架结构 -->
    <div class="todo-box">
      <header class="todo-hd">
        <input type="checkbox" v-model="allCompleted" class="checkbox" :value="true"/>
        <label class="toggle-all"></label>
        <input type="text" class="todo-hd_input" v-model="inputData" @keydown.enter="addTodo"/>
      </header>
      <section class="todo-bd">
        <!-- todo 事项  -->
        <d-item :item="item" v-for="(item, index) in currentTodoList" :key="index" :index="index" @on-delete="deleteHandler"></d-item>
      </section>
      <div class="todo-ft" v-show="allTodo.length > 0">
        <div class="todo-left">
          {{currentTodoList.length}} Item Left
        </div>
        <div class="list-btn">
          <d-button :active="current == 1" @click="current = 1">All</d-button>
          <d-button :active="current == 2" @click="current = 2">Active</d-button>
          <d-button :active="current == 3" @click="current = 3">Completed</d-button>
        </div>
        <div class="del-all" @click="allClear">Clear completed</div>
      </div>
    </div>
  </div>
</template>

<script>
import DItem from './components/d-item'
import DButton from './components/d-button'
let numID = 1
export default {
  name: 'todo',
  components: {
    DItem, DButton
  },
  data () {
    return {
      current: 2,
      allCompleted: false,
      inputData: '',
      allTodo: [], // 全部todo列表
      completedList: [], //  已完成的
      todoList: [] // 未完成的
    }
  },
  watch: {
    allCompleted (val) {
      return this.allTodo.map(item => {
        item.completed = val
        return item
      })
    }
  },
  computed: {
    // eslint-disable-next-line vue/return-in-computed-property
    currentTodoList () {
      // 当前显示列表 data中只需要声明一个allTodo 即可
      if (this.current === 1) {
        return this.allTodo
      } else if (this.current === 2) {
        return this.allTodo.filter(item => {
          return !item.completed
        })
      } else if (this.current === 3) {
        return this.allTodo.filter(item => {
          return item.completed
        })
      }
    }
    // ,currentTodo2 () {
    //   // 需要data中声明 completedList 和 todoList 用于存储完成和未完成的任务列表
    //   if (this.current === 1) {
    //     return [...this.todoList, ...this.completedList]
    //   } else if (this.current === 2) {
    //     return this.todoList
    //   } else if (this.current === 3) {
    //     return this.completedList
    //   }
    // }
  },
  methods: {
    // 添加 todo 项
    addTodo () {
      // 没有输入值时不进行后面操作
      if (!this.inputData) return
      this.allTodo.push({ value: this.inputData, completed: false, id: numID++ })
      // 添加成功后清空input框
      this.inputData = ''
    },
    // 删除项目
    deleteHandler (val) {
      this.allTodo = this.allTodo.filter(item => {
        return item.id !== val.id
      })
    },
    allClear () {
      this.allTodo = []
    }
  }
}
</script>

<style lang="less" scoped>
.page{
  background: #f5f5f5;
  color: #4d4d4d;
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
}
.todo-box {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.todo-hd{
  display: block;
  height: 65px;
  display: flex;
  box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
  align-items: center;
  .toggle-all{
    flex-basis: 60px;
    height: 34px;
    -webkit-transform: rotate(90deg);
    transform: rotate(90deg);
    &::before{
      content: '❯';
      font-size: 22px;
      color: #e6e6e6;
      padding: 10px 27px 10px 27px;
    }
  }
  .todo-hd_input{
    flex-grow: 1;
    padding: 16px 16px 16px 0;
    border: none;
    background: rgba(0, 0, 0, 0.003);
    line-height: 33px;
    width: 100%;
    outline: none;
    font-size: 24px;
    font-weight: 300;
    .edit{
      position: relative;
      margin: 0;
      width: 100%;
      font-size: 24px;
      font-family: inherit;
      font-weight: inherit;
      line-height: 1.4em;
      border: 0;
      color: inherit;
      padding: 6px;
      border: 1px solid #999;
      box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
      box-sizing: border-box;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }
  }
}

.todo-ft{
  color: #777;
  padding: 10px 15px;
  height: 20px;
  text-align: center;
  border-top: 1px solid #e6e6e6;
  display: flex;
  justify-content: space-between;
  // 底部阴影
  &::before {
    content: '';
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 50px;
    overflow: hidden;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6, 0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6, 0 17px 2px -6px rgba(0, 0, 0, 0.2);
  }

  // 清空按钮
  .del-all{
    position: relative;
    z-index: 10;
    &:hover{
      text-decoration: underline;
    }
  }
}

.checkbox{
  width: 50px;
  height: 60px;
  position: absolute;
  z-index: 10;
  opacity: 0;   // 隐藏原有选项框
  &:checked + .toggle-all::before{
    color: black;
  }
}

</style>
