<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('taches')}}</f7-nav-center>
    </f7-navbar>
    <f7-block>
      <!--{{todos }}-->
      <!--<todos v-model="todos"></todos>-->
      <todos></todos>
      <!--<button @click="addTodo">Ajouter une tache</button>!-->
  </f7-block>
    <f7-block>
      <f7-list media-list>
      <f7-list-item swipeout v-for="(value, key) in this.$db('taches')"  :title="value.libniv" :subtitle="value.note" :text="value.description" @swipeout:deleted="supprimer(key)">
        <f7-swipeout-actions>
          <f7-swipeout-button close color="blue" @click="SetEdit(key)">{{$lang('editer')}}</f7-swipeout-button>
          <f7-swipeout-button v-show="value.edit==''" delete>{{$lang('supprimer')}}</f7-swipeout-button>
        </f7-swipeout-actions>
      </f7-list-item>
    </f7-list>
  </f7-block>
  </f7-page>
</template>
<script>
  import Todos from './components/Todos'
  export default {
    data () {
      return {
        todos: [{
          name: 'Demo',
          completed: true
        }]
      }
    },
    watch: {
      value (value) {
        this.todos = value
      }
    },
    methods: {
      addTodo () {
        this.todos.push({
          name: 'Jean',
          completed: false
        })
      }
    },
    components: {
      Todos
    }
}
</script>
