<template>
  <ul>
    <SearchAndAdd @addTodo="addTodo" />
  </ul>
  <hr />
  <ul>
    <ToDoItem @remove="removeTodo(index)" v-for="(todo, index) in todos" :key="index" :content="todo.content" />
  </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script lang="ts">
import { ref } from 'vue';

import SearchAndAdd from './SearchAndAdd.vue';
import ToDoItem from './ToDoItem.vue';
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  components: {
    SearchAndAdd,
    ToDoItem,
  },
  name: 'ToDo',
  setup () {
       
      const defaultData = [{
          content: 'Write a blog post'
      }]
      const todosData = JSON.parse(localStorage.getItem('todos') as any) || defaultData; 
      const todos = ref(todosData);
      function addTodo (newTodo:any) {
        if(newTodo){
          todos.value.push({
              content: newTodo
          });
          saveData();
        }
      }
      
      function removeTodo (index: any) {
          todos.value.splice(index, 1);
          saveData();
      }
      function saveData () {
          const storageData = JSON.stringify(todos.value);
          localStorage.setItem('todos', storageData);
      }
      return {
          todos,
          addTodo,
          removeTodo,
          saveData
      }
  }
})
</script>

<style lang="scss">
ul{
  list-style: none;
  padding: 0;
}
li{
  width: 92%;  
  border-radius: 5px;
  padding: 10px 10px 10px 10px;
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  margin-bottom: 20px;
}
span{
  padding: 0;
  margin: 5px 0 0 0;
}
h1{
	text-align: center;
  font-size: 30px;
}
h1, h2, h4{
	font-weight: normal;
}
h2{
	border-bottom: 1px solid $primaryCol;
  font-size: 20px;
  padding-bottom: 10px;
}
</style>
