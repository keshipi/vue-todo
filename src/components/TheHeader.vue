<template>
  <header class="header">
    <h1>todos</h1>
    <input type="text" class="new-todo" v-model="newTodo" @keyup.enter="addTodo">
  </header>
</template>

<script>
export default {
  name: 'TheHeader',
  data() {
    return {
      newTodo: ''
    }
  },
  methods: {
    addTodo: function() {
      const todo = this.newTodo && this.newTodo.trim()
      if (!todo) {
        return
      }
      this.$emit('add', todo)
      this.newTodo = ''
    }
  }
}
</script>

<style scoped>
.new-todo {
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
  padding: 16px 16px 16px 60px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
</style>
