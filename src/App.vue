<script lang="ts">
  import { defineComponent } from "vue";
  import AppHeader from './components/AppHeader.vue'
  import AppFilters from './components/AppFilters.vue'
  import AppTodoList from './components/AppTodoList.vue'
  import AppAddTodo from './components/AppAddTodo.vue'
  import AppFooter from './components/AppFooter.vue'
  import { Todo } from "./types/Todo";
  import { Filter } from "./types/Filter";

  interface State {
    todos: Todo[],
    activeFilter: Filter
  }

  interface Stats {
      active: number,
      done: number
  }

  export default defineComponent({
    components: {
      AppHeader,
      AppFilters,
      AppTodoList,
      AppAddTodo,
      AppFooter
    },
    data(): State {
      return {
        todos: [
          {id: 0, text: 'Learn the basics of Vue3', completed: true},
          {id: 1, text: 'Learn the basics of Typescript', completed: false},
          {id: 2, text: 'Subscribe to the channel', completed: false}
        ],
        activeFilter: 'All'
      }
    },
    methods: {
      toggleTodo(id: number) {
        const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

        if (targetTodo) {
          targetTodo.completed = !targetTodo.completed
        }
      },
      removeTodo(id: number) {
        this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
      },
      addTodo(todo: Todo) {
        this.todos.push(todo)
      },
      setFilter(filter: Filter) {
        this.activeFilter = filter
      }
    },
    computed: {
      filteredTodos(): Todo[] {
        switch(this.activeFilter) {
          case 'Active':
            return this.todos.filter(todo => !todo.completed)
          case 'Done':
            return this.todos.filter(todo => todo.completed)
          case 'All': 
          default:
            return this.todos
        }
      },
      stats(): Stats {
        return {
          active: this.todos.filter(todo => !todo.completed).length,
          done: this.todos.filter(todo => todo.completed).length
        }
      }
    }
  })

</script>

<template>
  <AppHeader />

  <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />

  <main class="app-main">
    <AppTodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo" />

    <AppAddTodo @add-todo="addTodo" />
  </main>

  <AppFooter :stats="stats" />

</template>