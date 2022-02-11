<template>
<li @mouseover="upHere = true" @mouseleave="upHere = false">
  <div class="item-container">
    <div v-show="checkIfAlreadyExists" class="icon icon-check" ></div>
    <div>
      <div>{{ content }}</div>
      <div><span v-if="checkIfAlreadyExists">Exact match </span>#{{idx}}</div>
    </div>
  </div>
  <div class="item-container">
    <div>{{elapsedTime}}</div>
	  <div v-show="upHere" class="icon icon-trash" @click="$emit('remove')"></div>
  </div>
</li>
<hr>
</template>

<script>
import { ref } from 'vue';
import { formatDistance } from 'date-fns'
import { computed, defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'ToDoItem',
  props: {
    content: String,
    addedDate: String,
    idx:Number,
    checkIfAlreadyExists:Boolean,
  },
  emits: ['remove'],
	setup(props){
		const upHere=ref(false);
    const elapsedTime=computed(()=>{
        return formatDistance(new Date(props.addedDate), new Date(), { addSuffix: true })
    })
		return {
			upHere,
      elapsedTime,
		}
	}	
})

</script>

<style  lang="scss" scoped>
li{
  background: $backCol;
  cursor: pointer;
}
li:hover{
  background: $backFocusCol;
}
span{
  color: $greenCol;
}
hr {
  margin: 0px;
}
.item-container{
  display: flex;
  align-items: center;
}
.icon {
  height: 20px;
  width: 20px;
  margin:5px;
  -webkit-mask-size: contain;
  mask-size: contain;
  -webkit-mask-position: center;
  mask-position: center;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}
.icon-trash {
  background-color: $redCol;
  mask-image: url("../assets/trash.svg");
}
.icon-check {
  background-color: $greenCol;
  mask-image: url("../assets/check.svg");
}
</style>
