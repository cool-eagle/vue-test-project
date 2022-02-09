<template>
  <li >
      <input v-model="newTodo" name="newTodo" autocomplete="off" placeholder="Search or Add..." />

    
      <div v-if="isShow" class="icon icon-cancel" @click="cancelTodo"></div>
      <div v-if="isShow" class="icon icon-add" @click="addTodo"></div>
      
    
  </li>
</template>

<script lang="ts">
import { ref } from 'vue';
import { computed, defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'SearchAndAdd',
	emits: ['addTodo'],
  setup (props,{emit}) {
    const newTodo = ref(''); 
    const upHere=ref(false);
    const addTodo=()=>{
			emit("addTodo",newTodo.value)
			newTodo.value='';
		}
    const cancelTodo=()=>{
      newTodo.value='';
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
    }
  }
})
</script>

<style lang="scss">
li{
  background: $quinaryCol;
  cursor: pointer;
}

input {
	width: 90%;
  height: 30px;
  border: none;
  background: transparent;
  border-radius: 5px;
  color: $primaryCol;
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
  background-color: #00f0f0;
}
.icon-cancel {
  mask-image: url("../assets/cancel.svg");
  background-color: red;
}
</style>
