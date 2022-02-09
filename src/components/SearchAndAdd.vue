<template>
  <form @submit.prevent="addTodo">
    <label>New ToDo</label>
    <input v-model="newTodo" name="newTodo" autocomplete="off" />
    <button>Add ToDo</button>
  </form>
</template>

<script lang="ts">
import { ref } from 'vue';
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'SearchAndAdd',
	emits: ['addTodo'],
  setup (props,{emit}) {
    const newTodo = ref(''); 
    const addTodo=()=>{
			emit("addTodo",newTodo.value)
			newTodo.value='';
		}
    return {
      newTodo,
			addTodo,
    }
  }
})
</script>

<style lang="scss">

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
