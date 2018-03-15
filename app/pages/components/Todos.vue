<template>
  <section class="todoapp">
    <header class="header">
      <input type="text" class="new-todo" placeholder="Ajouter une tache" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input type="checkbox" class="toggle-all" v-model="allDone" >
      <ul class="todo-list">
        <li class="todo" v-for="todo in filtererdTodos" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="donEdit" @keyup.esc="cancelEdit" v-focus="todo === editing" @blur="donEdit">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <ul class="filters">
      <li>
        <a href="#" :class="{selected : filter==='all'}" @click.prevent=" filter = 'all'">Toutes</a>
        <a href="#" :class="{selected : filter==='todo'}" @click.prevent=" filter = 'todo'">A faire</a>
        <a href="#" :class="{selected : filter==='done'}" @click.prevent=" filter = 'done'">Faites</a>
      </li>
    </ul>
    <f7-block>
        <span class="todo-count">
          <strong>{{remaining}}</strong>
          Tache a faire
        </span>
      <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les finis</button>
    </f7-block>
    </footer>
  </section>
</template>
<script>
  export default{
    props: {
      value: {type: Array, default () { return [] }}
    },
    data () {
      this.$db('todos') ? this.value = this.$db('todos') : this.value = []
      return {
        todos: this.value,
        newTodo: '',
        filter: 'all',
        editing: null,
        oldTodo: ''
      }
    },
    computed: {
      hasTodos () {
        return this.todos.length > 0
      },
      allDone: {
        get () {
          return this.remaining === 0
        },
        set (value) {
          this.todos.forEach(todo => {
            todo.completed = value
          })
          this.$fireDB('todos').forEach(todo => {
            todo.completed = value
          })
        }
      },
      remaining () {
        return this.todos.filter(todo => !todo.completed).length
      },
      completed () {
        return this.todos.filter(todo => todo.completed).length
      },
      filtererdTodos () {
        if (this.filter === 'todo') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'done') {
          return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      }
    },
    methods: {
      deleteTodo (todo) {
        this.todos = this.todos.filter(i => i !== todo)
        this.emit('input', this.todos)
        this.$fireDB('todos').set(this.todos);
      },
      deleteCompleted () {
        this.todos = this.todos.filter(todo => !todo.completed)
        this.emit('input', this.todos)
        this.$fireDB('todos').set(this.todos);
      },
      editTodo (todo) {
        this.oldTodo = this.oldTodo
        this.editing = todo
        this.$fireDB('oldTodo').set(this.oldTodo);
        this.$fireDB('editing').set(todo);

      },
      donEdit () {
        this.editing = null
        this.$fireDB('editing').set(null);
      },
      cancelEdit () {
        this.editing.name = this.oldTodo
        this.$fireDB('editing.name').set(this.oldTodo);
        this.donEdit()
      },
      addTodo () {
        this.todos.push({
          completed: false,
          name: this.newTodo
        })
        this.$fireDB('todos').set(this.todos);
        this.$fireDB('newTodo').set('');
        this.newTodo = ''
      }
    },
    directives: {
      focus (el, value) {
        if (value) {
          Vue.nextTick(_ => {
            el.focus()
          })
        }
      }
    },
    created() {
      this.$db('todos').on('value', (snap) => {
      this.$fireDB('todos', snap.val())
    })
    }
  }

</script>
<style src="./todos.css">

</style>
