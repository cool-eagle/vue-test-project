<template>
  <li >
      <input 
        v-model="newTodo" 
        @input="searchChange" 
        @keypress.enter="addTodo" 
        @keydown.esc="cancelTodo" 
        name="newTodo" 
        autocomplete="on" 
        placeholder="Search or Add..." 
      />
      <div 
        v-if="isShow" 
        class="icon icon-cancel" 
        @click="cancelTodo"
      ></div>
      <div 
        v-if="isShow && !isAlreadyExists" 
        class="icon icon-add" 
        @click="addTodo"
      ></div>
    
  </li>
</template>

<script lang="ts">
import { ref } from 'vue';
import { computed, defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'SearchAndAdd',
  props: {isAlreadyExists:Boolean},
	emits: ['addTodo', 'searchChange'],
  setup (props,{emit}) {
    const newTodo = ref(''); 
    const upHere=ref(false);
    const addTodo=()=>{
      if(!props.isAlreadyExists){
        emit("addTodo",newTodo.value);
        newTodo.value='';
      }
		}
    const searchChange=()=>{
			emit("searchChange",newTodo.value)
		}
    const cancelTodo=()=>{
      newTodo.value='';
      emit("searchChange",newTodo.value)
    }
    const isShow=computed(()=>{
      if(newTodo.value){
        upHere.value=true;
      } else {
        upHere.value=false;
      }
      return upHere.value;
    })
    return {
      isShow,
      newTodo,
			addTodo,
      cancelTodo,
      searchChange
    }
  }
})
</script>

<style lang="scss" scoped>
li{
  background: $backCol;
}
input {
	width: 90%;
  height: 30px;
  border: none;
  background: transparent;
  border-radius: 5px;
  color: $text;
  padding: 0 10px;
}
.icon {
  height: 30px;
  width: 30px;
  -webkit-mask-size: contain;
  mask-size: contain;
  -webkit-mask-position: center;
  mask-position: center;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}
.icon-add {
  mask-image: url("../assets/add.svg");
  background-color: $greenCol;
}
.icon-cancel {
  mask-image: url("../assets/cancel.svg");
  background-color: $redCol;
}
</style>
