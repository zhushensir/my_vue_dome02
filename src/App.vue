<template>
<div class="todo-container">
    <div class="todo-wrap">
      <todo-header :addTodo="addTodo"/>
      <todo-list :todos="todos" :deleteTodo="deleteTodo"/>
      <todo-footer :todos="todos" :deleteCompleteTodos="deleteCompleteTodos" :selectAllTodos="selectAllTodos"/>
    </div>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data () {
    return {
      /** 从Localstorage读取todos，深度监视 */
      todos: JSON.parse(window.localStorage.getItem('todos_key') || '[]')
    }
  },

  methods: {
    addTodo (todo) {
      /** unshift 就是给一个数组添加一个对象进去 */
      this.todos.unshift(todo)
    },
    deleteTodo (index) {
      this.todos.splice(index, 1)
    },
    /** 删除所有选中的 */
    deleteCompleteTodos () {
      this.todos = this.todos.filter(todo => !todo.complete)
    },
    /** 实现全选或者全不选 */
    selectAllTodos (isCheck) {
      this.todos.forEach(todo => (todo.complete = isCheck))
    }
  },
  /** 深度监视 */
  watch: {
    todos: {
      deep: true,
      handler: function (Value) {
        /** 将todos最新的值保存到localstorage里面,需要转成JSON */
        window.localStorage.setItem('todos_key', JSON.stringify(Value))
      }
    }
  },

  components: {
    TodoHeader,
    TodoList,
    TodoFooter
  }
}

</script>

<style>

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>
