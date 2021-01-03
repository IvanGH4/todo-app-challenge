<template>
  <div :class="[container, {'dark': isDark}]">
    <div :class="[head, {'dark-desk-bg': isDark}, {'dark-mobile-bg': isDark && isMobile}, {'light-desk-bg': !isDark}, {'light-mobile-bg': !isDark && isMobile}]"> <!-- aca va bg inline -->
      
      <div class="wrapper">
        <div class="title">
          <h1>todo</h1>
          <img v-if="isDark" src="@/assets/images/icon-sun.svg" alt="#" @click="setLightMode" />
          <img v-else src="@/assets/images/icon-moon.svg" alt="#" @click="setDarkMode" />
        </div>

        <form :class="[form, {'form-dark-bg': isDark}, {'light-form-bg': !isDark}]" @submit.prevent="addTodo(task)">
          <div :class="[circle, {'border-dark': isDark}, {'border-light': !isDark}]"></div>
          <input :class="[input, {'dark-input': isDark}]" type="text" placeholder="Create a new todo..." v-model="task" />
        </form>
      </div>
    </div>  
      <!-- List of Todos -->
    <div class="todos-container">
      <div class="todos-wrapper">
        <div :class="[todoForm, {'form-dark-bg': isDark}, {'white-bg': !isDark}]">

          <div class="todo border-bottom" v-for="(todo, idx) in filterTodos" :key="idx">
            <div @click="markComplete(idx)" :class="[todoCircle, {'border-dark': isDark}, {'border-light': !isDark}, {'bg-check': todo.completed}]">
              <img v-if="todo.completed" src="@/assets/images/icon-check.svg" alt="#" />
            </div>
            <div :class="[text, {'dark-text': isDark}, {'light-text': !isDark}, {'completed': todo.completed}]">
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
            <div>
              <button @click="clearCompleted" class="btn">Clear Completed</button>
            </div>
          </div>

      </div>
      <div :class="[btns, {'form-dark-bg': isDark}, {'white-bg': !isDark}]">
          <button @click="filter = 'all'" :class="[btn, {'active-btn': filter == 'all'}]">All</button>
          <button @click="filter = 'active'" :class="[btn, {'active-btn': filter == 'active'}]">Active</button>
          <button @click="filter = 'completed'" :class="[btn, {'active-btn': filter == 'completed'}]">Completed</button>
      </div>
      </div>
    </div> 
  </div>
</template>

<script>

export default {
  name: 'Home',
  props: {
    head: {
      type: String,
      default: 'head'
    },
    container: {
      type: String,
      default: 'container'
    },
    input: {
      type: String,
      default: 'todo-input'
    },
    circle: {
      type: String,
      default: 'circle'
    },
    form: {
      type: String,
      default: 'todo-form'
    },
    // todos props
    btn: {
      type: String,
      default: 'btn'
    },
    btns: {
      type: String,
      default: 'apart-btns'
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
      windowWidth: null,
      isDark: true,
      task: '',
      todos: [],
      complete: false,
      filter: 'all',
    }
  },
  mounted() {
    window.addEventListener('resize', this.getWindowSize);
    this.$on("hook:beforeDestroy", () => {
      window.removeEventListener('resize', this.getWindowSize);
    });
    this.getLSTodos();
  },
  methods: {
    getWindowSize() {
      this.windowWidth = document.documentElement.clientWidth;
    },
    addTodo(todo) {
      this.todos.push({
        text: todo,
        completed: false,
      });
      localStorage.setItem('todos', JSON.stringify(this.todos));
      this.task = '';
      this.getLSTodos();
    },
    setDarkMode() {
      this.isDark = true;
    },
    setLightMode() {
      this.isDark = false;
    },
    // todos functions
    markComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    deleteTodo(idx) {
      this.todos.splice(idx, 1)
      this.removeLSTodo(idx);
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    getLSTodos() {
      if(localStorage.getItem('todos') === null) {
        return
      } else {
        this.todos = JSON.parse(localStorage.getItem('todos'));
      }
    },
    removeLSTodo(idx) {
      if(localStorage.getItem('todos') === null) {
        return
      } else {
        this.todos = JSON.parse(localStorage.getItem('todos'));
        this.todos.splice(idx, 1);
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
      // console.log(todo);
    }
  },
  computed: {
    isMobile() {
      return this.windowWidth !== null && this.windowWidth <= 400;
    },
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
