<template>
  <div class='container'>
    <div style='flex: 1'>
      <CreateTodo @addItem='addTodo' />
      <ListTodo :todos='todos' @removeTodo='removeTodo' />
    </div>
    <footer>
      <span>{{ done }} done - {{ remaining }} uncompleted</span>
    </footer>
  </div>
</template>

<script>
import CreateTodo from './CreateTodo'
import ListTodo from './ListTodo'
import eventBus from '@/eventBus'
import axios from 'axios'

export default {
  name: 'TodoList',
  data () {
    return {
      todos: []
    }
  },
  components: {
    CreateTodo,
    ListTodo
  },
  computed: {
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    done () {
      return this.todos.filter(todo => todo.completed).length
    }
  },
  created () {
    eventBus.$on('removeTodo', this.removeTodo.bind(this))
    axios
      .get(`https://todolist-express-api.herokuapp.com/todo`)
      .then(response => {
        if (response.data.status === 'success') {
          this.todos = response.data.todos
        } else this.todos = []
      })
      .catch(e => {
        this.todos = []
      })
  },
  methods: {
    addTodo (value) {
      if (value.trim() === '') return

      axios
        .post('https://todolist-express-api.herokuapp.com/todo', {
          text: value,
          completed: false
        })
        .then((response) => {
          console.log(response.data)
          if (response.data.status === 'success') {
            this.todos.push(response.data.todo)
          } else {
            throw new Error('error')
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    removeTodo (todo) {
      axios
        .delete(`https://todolist-express-api.herokuapp.com/todo/${todo.id}`)
        .then((response) => {
          console.log(response.data)
          if (response.data.status === 'success') {
            this.todos = this.todos.filter(x => x.id !== todo.id)
          } else {
            throw new Error('error')
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style lang='scss'>
@import './style.scss'
</style>
