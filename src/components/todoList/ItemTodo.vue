<template>
  <div>
    <label class='material-checkbox'>
      <input type='checkbox' v-model="todo.completed" @change='changeCompelted(todo)' />
      <span></span>
    </label>
    <div
      class='text'
      :class='{completed: todo.completed}'
      v-if='!editing'
      @dblclick='dbClickEdit(todo)'
    >{{ todo.text }}</div>
    <!-- Show Text Box -->
    <div class='input__div' v-else>
      <div class='input__wrapper'>
        <input
          type='text'
          v-model='editTodoCache'
          @keyup.enter='editTodo(todo)'
          @keyup.esc='cancelEdit(todo)'
          @blur='cancelEdit(todo)'
        />
      </div>
      <div class='border'></div>
    </div>
    <label class='material-delete'>
      <button @click='removeTodo(todo)'>X</button>
      <span></span>
    </label>
  </div>
</template>

<script>
import eventBus from '@/eventBus'
import axios from 'axios'
export default {
  props: ['todo'],
  data () {
    return {
      editTodoCache: this.todo.text,
      editing: false
    }
  },
  methods: {
    editTodo (todo) {
      todo.text = this.editTodoCache
      this.editing = false
      axios
        .put(`https://todolist-express-api.herokuapp.com/todo/${todo.id}`, {
          text: todo.text,
          completed: todo.completed
        })
        .then(response => {
          if (response.data.status === 'error') {
            throw new Error('error')
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    cancelEdit (todo) {
      todo.text = this.editTodoCache
      this.editing = false
    },
    dbClickEdit (todo) {
      this.editing = true
    },
    changeCompelted (todo) {
      axios
        .put(`https://todolist-express-api.herokuapp.com/todo/${todo.id}`, {
          text: todo.text,
          completed: todo.completed
        })
        .then(response => {
          if (response.data.status === 'error') {
            throw new Error('error')
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    removeTodo (todo) {
      eventBus.$emit('removeTodo', todo)
    }
  }
}
</script>
