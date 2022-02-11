<template>
  <div :class="$style.container">
    <div :class="$style.somethingListBox"></div>
    <div :class="$style.mainListBox">
      <ul>
        <SearchAndAdd 
          @addTodo="addTodo" 
          @searchChange="searchChange" 
          :isAlreadyExists="isAlreadyExists"
        />
      </ul>
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
    <div :class="$style.buttonListBox">
      <ul>
        <li>
          <SortValue 
            @sortValueClick="sortValueClick"
          />
        </li>
        <li>
          <SortDate 
            @sortDateClick="sortDateClick"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';
import { computed, defineComponent } from '@vue/runtime-core';
import _ from 'lodash';

import SearchAndAdd from './SearchAndAdd.vue';
import ToDoItem from './ToDoItem.vue';
import SortValue from './SortValue.vue';
import SortDate from './SortDate.vue';

export default defineComponent({
  components: {
    SearchAndAdd,
    ToDoItem,
    SortValue,
    SortDate
  },
  name: 'ToDo',
  setup () {
      const todosData = JSON.parse(localStorage.getItem('todos') as any) || []; 
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
          isAlreadyExists.value=todos.value.some(
            (todo:any) => todo.content.toLowerCase().trim() === newTodo.toLowerCase().trim()
          );
      }      

      const filteredList=computed(()=>{
        return todos.value.filter(
          (todo:any) => todo.content.toLowerCase().includes(searchText.value.toLowerCase())
        );
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

<style lang="scss" module>
  .container {
    display:flex;
    justify-content: space-around;
  }
  .mainListBox {
    flex-basis:30%;
  }
  .somethingListBox {
    flex-basis:20%;
  }
  .buttonListBox {
    flex-basis:20%;
  }
ul{
  list-style: none;
  padding: 0;
}
ul>hr:last-child{
  display: none;
}
li{
  width: 100%;
  border-radius: 5px;
  padding: 10px 10px 10px 10px;
  display: flex;
  justify-content: space-between;
}
button{
  width: 100%;
  margin-top: 10px;
  margin-right: 30%;
  padding: 1em;
  color: $text;
  background: $backCol;
  border-radius: 5px;
  border: none;
  text-align: start;
  cursor: pointer;
}
button:hover{
  background: $backFocusCol;
}
</style>
