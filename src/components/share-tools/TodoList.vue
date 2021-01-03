<template>
<div class="todos-container">
  <div class="todos-wrapper">
    <div :class="[todoForm, {'form-dark-bg': dark}, {'white-bg': !dark}]">

      <div class="todo border-bottom" v-for="(todo, idx) in filterTodos" :key="idx">
        <div @click="markComplete(idx)" :class="[todoCircle, {'border-dark': dark}, {'border-light': !dark}]"></div>
        <div :class="[text, {'dark-text': dark}, {'light-text': !dark}, {'completed': todo.completed}]">
          <p>{{todo.text}}</p>
        </div>
        <div class="delete-todo" @click="deleteTodo(idx)">
          <img src="@/assets/images/icon-cross.svg" alt="delete" />
        </div>
      </div>
      <div class="options">
        <div class="quantity">
          <span>{{todos.length}} todos</span>
        </div>
        <div class="btn-box">
          <button @click="filter = 'all'" :class="[btn, {'active-btn': filter == 'all'}]">All</button>
          <button @click="filter = 'active'" :class="[btn, {'active-btn': filter == 'active'}]">Active</button>
          <button @click="filter = 'completed'" :class="[btn, {'active-btn': filter == 'completed'}]">Completed</button>
        </div>
        <!-- <div>
          <button @click="clearCompleted" class="btn">Clear Completed</button>
        </div> -->
      </div>

    </div>
  </div>
</div>
</template>

<script>
export default {
    name: 'TodoList',
    props: {
        myTodos: {
          type: Array
        },
        dark: {
          type: Boolean,
          default: true,
        },
        btn: {
          type: String,
          default: 'btn'
        },
        text: {
          type: String,
          default: 'todo-text'
        },  
        todoInput: {
          type: String,
          default: 'todo-input'
        },
        todoCircle: {
          type: String,
          default: 'circle'
        },
        todoForm: {
          type: String,
          default: 'list-form'
        }
    },
    data() {
      return {
        complete: false,
        filter: 'all',
        todos: this.myTodos
      }
    },
    methods: {
      markComplete(index) {
        this.todos[index].completed = !this.todos[index].completed;
      },
      deleteTodo(idx) {
        this.todos.splice(idx, 1)
      }
    },
    computed: {
      filterTodos() {
        if (this.filter == 'all') {
          return this.todos
        } else if (this.filter == 'active') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter == 'completed') {
          return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      },
    }
}
</script>