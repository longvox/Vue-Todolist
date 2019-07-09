<template>
  <div>
     <div class='input__div'>
        <div class='input__wrapper'>
          <input type='text' placeholder='Things needs to be done.' v-model='newTodo' @keyup.enter='addTodo' />
        </div>
        <div class='border'></div>
      </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newTodo: ''
    }
  },
  methods: {
    addTodo () {
      this.$emit('addItem', this.newTodo.toString())
      this.newTodo = ''
    }
  }
}
</script>
