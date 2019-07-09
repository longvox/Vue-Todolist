<template>
  <div>
    <div class='todo-list'>
      <!-- Loop Over All Todos -->
      <div v-for='todo in todos' :key='todo.id' class='list'>
        <ItemTodo
          :todo='todo'/>

      </div>
    </div>
  </div>
</template>

<script>
import ItemTodo from './ItemTodo'

export default {
  props: ['todos'],
  data () {
    return {
      editTodoCache: ''
    }
  },
  components: {
    ItemTodo
  }
}
</script>
