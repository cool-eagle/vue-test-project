<template>

  <h1>ToDo App</h1>
  <form @submit.prevent="addTodo()">
    <label>New ToDo</label>
    <input v-model="newTodo" name="newTodo" autocomplete="off" />
    <button>Add ToDo</button>
  </form>

  <h2>ToDo List</h2>
  <ul>
    <ToDoItem @remove="removeTodo(index)" v-for="(todo, index) in todos" :key="index" :content="todo.content" />
  </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script lang="ts">
import { ref } from 'vue';

import ToDoItem from './ToDoItem.vue';
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  components: {
      ToDoItem,
    },
    name: 'ToDo',
    setup () {
        const newTodo = ref(''); 
        const defaultData = [{
            content: 'Write a blog post'
        }]
        const todosData = JSON.parse(localStorage.getItem('todos') as any) || defaultData; 
        const todos = ref(todosData);
        function addTodo () {
            if (newTodo.value) {
                todos.value.push({
                    content: newTodo.value
                });
                newTodo.value = '';
            }
            saveData();
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
            newTodo,
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
ul button{
  background: purple;
  color: $primaryCol;
  border-radius: 3px;
  border: none;
  font-size: 13px;
  padding: 5px 4px;
  cursor: pointer;
}
ul button:hover{
  background: $secondaryCol;
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
label{
  font-size: 15px;
}
form{
  width: 100%;
}
form button{
  width: 100%;
  height: 30px;
  margin-top: 10px;
  color: $primaryCol;
  background: $tercialyCol;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}
form button:hover{
  background: $secondaryCol;
}
input{
	width: 93%;
  height: 30px;
  border: 1px solid $primaryCol;
  background: none;
  border-radius: 5px;
  color: $primaryCol;
  padding: 0 10px;
}
</style>
