<template>
<li @mouseover="upHere = true" @mouseleave="upHere = false">
  <div>
    <div v-show="checkIfAlreadyExists" class="icon icon-check" ></div>
    <div>{{ content }}</div>
    <div><span v-if="checkIfAlreadyExists">Exact match </span>#{{idx}}</div>
  </div>
	<div v-show="upHere" class="icon icon-trash" @click="$emit('remove')"></div>
</li>
</template>

<script>
import { ref } from 'vue';
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'ToDoItem',
  props: {
    content: String,
    idx:Number,
    checkIfAlreadyExists:Boolean,
  },
  emits: ['remove'],
	setup(){
		const upHere=ref(false);
		return {
			upHere,
		}
	}	
})

</script>

<style scoped lang="scss">
li{
  background: $quinaryCol;
  cursor: pointer;
}
li:hover{
  background: $quaternalyCol;
}
span{
  color: #008080;
}
.icon {
  height: 20px;
  width: 20px;
  -webkit-mask-size: contain;
  mask-size: contain;
  -webkit-mask-position: center;
  mask-position: center;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}
.icon-trash {
  background-color: red;
  mask-image: url("../assets/trash.svg");
}
.icon-check {
  background-color: #008080;
  mask-image: url("../assets/check.svg");
}
</style>
