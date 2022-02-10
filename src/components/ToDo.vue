<template>
  <div>
    <ul>
      <SearchAndAdd @addTodo="addTodo" @searchChange="searchChange" :isAlreadyExists="isAlreadyExists"/>
    
      <SortValue @sortValueClick="sortValueClick"></SortValue>
      <SortDate @sortDateClick="sortDateClick"></SortDate>
    </ul>
  </div>
  <div>
    <hr />
    <ul>    
      <ToDoItem 
        @remove="removeTodo(index)" 
        v-for="(todo, index) in filteredList" 
        :key="index" 
        :content="todo.content" 
        :addedDate="todo.addedDate" 
        :idx="index+1" 
        :checkIfAlreadyExists="checkIfAlreadyExists(todo)" 
      />
    </ul>
    
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';

import SearchAndAdd from './SearchAndAdd.vue';
import ToDoItem from './ToDoItem.vue';
import SortValue from './SortValue.vue';
import SortDate from './SortDate.vue';
import _ from 'lodash';

import { computed, defineComponent } from '@vue/runtime-core';

export default defineComponent({
  components: {
    SearchAndAdd,
    ToDoItem,
    SortValue,
    SortDate
  },
  name: 'ToDo',
  setup () {
       
      const defaultData = [{
          content: 'Write a blog post',
          addedDate: new Date()
      }]
      const todosData = JSON.parse(localStorage.getItem('todos') as any) || defaultData; 
      const todos = ref(todosData);
      const searchText=ref('');
      let isAlreadyExists=ref(false);
      function addTodo (newTodo:any) {
        if(newTodo){
          todos.value.push({
              content: newTodo,
              addedDate: new Date()
          });
          saveData();
          searchText.value='';
        }
      }
      
      function searchChange (newTodo:any) {
          searchText.value=newTodo;
          isAlreadyExists.value=todos.value.some((todo:any) => todo.content.toLowerCase().trim() === newTodo.toLowerCase().trim());
      }      

      const filteredList=computed(()=>{
        return todos.value.filter((todo:any) => todo.content.toLowerCase().includes(searchText.value.toLowerCase()));
      });
      
      function removeTodo (index: any) {
          todos.value.splice(index, 1);
          saveData();
      }
      function sortValueClick () {
        todos.value=_.orderBy(todos.value, 'content', 'asc');       
      }
      function sortDateClick () {
        todos.value=_.orderBy(todos.value, function(todo) {return new Date(todo.addedDate)}, 'desc');       
      }
      function saveData () {
          const storageData = JSON.stringify(todos.value);
          localStorage.setItem('todos', storageData);
      }
      function checkIfAlreadyExists (todo:any) {
          return todo.content.toLowerCase() === searchText.value.toLowerCase()
      }
      return {
          todos,
          addTodo,
          removeTodo,
          saveData,
          filteredList,
          searchChange,
          searchText,
          sortValueClick,
          sortDateClick,
          checkIfAlreadyExists,
          isAlreadyExists,
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
button{
  width: 100%;
  height: 30px;
  margin-top: 10px;
  color: $text;
  background: $quinaryCol;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}
button:hover{
  background: $quaternalyCol;
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
