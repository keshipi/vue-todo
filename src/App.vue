<template>
  <div id="app">
    <section class="todoapp">
      <the-header @add="addTodo" />
      <todo-list :todoList="filteredTodos" :left="countActive" @removeTodo="removeTodo" @allDone="allDone" />
      <the-footer :todoList="todos" :left="countActive" :visibility="visibility" @filter="setVisibility" @removeCompleted="removeCompleted" />
    </section>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader'
import TodoList from './components/TodoList'
import TheFooter from './components/TheFooter'

var STORAGE_KEY = 'todos-vuejs-2.0'
var todoStorage = {
  fetch: function() {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function(todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}

const filters = {
  all: function(todos) {
    return todos
  },
  active: function(todos) {
    return todos.filter(function(todo) {
      return !todo.completed
    })
  },
  completed: function(todos) {
    return todos.filter(function(todo) {
      return todo.completed
    })
  }
}

export default {
  name: 'app',
  components: { TheHeader, TodoList, TheFooter },
  data() {
    return {
      todos: todoStorage.fetch(),
      visibility: 'all'
    }
  },
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos)
      },
      deep: true
    }
  },
  methods: {
    addTodo: function(todo) {
      this.todos.push({
        id: todoStorage.uid++,
        title: todo,
        completed: false
      })
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    },
    setVisibility: function(visibility) {
      this.visibility = visibility
    },
    removeCompleted: function() {
      this.todos = filters['active'](this.todos)
    },
    allDone: function(value) {
      this.todos.forEach(function(todo) {
        todo.completed = value
      })
    }
  },
  computed: {
    filteredTodos: function() {
      return filters[this.visibility](this.todos)
    },
    countActive: function() {
      return filters['active'](this.todos).length
    }
  }
}
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

button {
  margin: 0;
  padding: 0;
  border: 0;
  background: none;
  font-size: 100%;
  vertical-align: baseline;
  font-family: inherit;
  font-weight: inherit;
  color: inherit;
  -webkit-appearance: none;
  appearance: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
  line-height: 1.4em;
  background: #f5f5f5;
  color: #4d4d4d;
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 300;
}

:focus {
  outline: 0;
}

.hidden {
  display: none;
}

.todoapp {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.todoapp input::-webkit-input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp input::-moz-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp input::input-placeholder {
  font-style: italic;
  font-weight: 300;
  color: #e6e6e6;
}

.todoapp h1 {
  position: absolute;
  top: -155px;
  width: 100%;
  font-size: 100px;
  font-weight: 100;
  text-align: center;
  color: rgba(175, 47, 47, 0.15);
  -webkit-text-rendering: optimizeLegibility;
  -moz-text-rendering: optimizeLegibility;
  text-rendering: optimizeLegibility;
}
</style>