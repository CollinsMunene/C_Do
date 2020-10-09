<template>
  <div id="app">
    <h1>To-Do List</h1>
    <to-do-form @todo-added="addToDo"></to-do-form>
    <h2 id="list-summary">{{listSummary}}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in ToDoItems" v-bind:key="item.id">
      <to-do-item :label="item.label" :done="item.done" :id="item.id"
            @checkbox-changed="updateDoneStatus(item.id)"        
            @item-deleted="deleteToDo(item.id)"
            @item-edited="editToDo(item.id, $event)">
      </to-do-item>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue';
import ToDoForm from './components/ToDoForm';
import uniqueId from 'lodash.uniqueid'

export default {
  name: 'App',
  components:{
    ToDoItem,
    ToDoForm
  },
  data() {
    return {
      ToDoItems: [
        { id: uniqueId('todo-'), label: 'Learn Vue', done: false },
        { id: uniqueId('todo-'), label: 'Create a Vue project with the CLI', done: true },
        { id: uniqueId('todo-'), label: 'Have fun', done: true },
        { id: uniqueId('todo-'), label: 'Create a to-do list', done: false }
      ]
    };
  },
  methods: {
    addToDo(toDoLabel){
      this.ToDoItems.push({id:uniqueId('todo-'), label: toDoLabel, done: false});
      console.log("to-do added",toDoLabel);
    },
    updateDoneStatus(toDoId){
        const toDoToUpdate = this.ToDoItems.find(item => item.id === toDoId)
        //to be checked
        toDoToUpdate.done = !toDoToUpdate.done
    },
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex(item => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
    },
    editToDo(toDoId, newLabel) {
      const toDoToEdit = this.ToDoItems.find(item => item.id === toDoId);
      toDoToEdit.label = newLabel;
    }
  },
  computed: {
    listSummary(){
      const numberFinishedItems = this.ToDoItems.filter(item => item.done).length
      return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
