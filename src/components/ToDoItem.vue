<template id="to-do-item">
<li @mouseover="upHere = true" @mouseleave="upHere = false">  
      <span>{{ content }}</span>
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
.icon {
  height: 20px;
  width: 20px;
  background-color: red;
  -webkit-mask-size: contain;
  mask-size: contain;
  -webkit-mask-position: center;
  mask-position: center;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}
.icon-trash {
  mask-image: url("../assets/trash.svg");
}
</style>
