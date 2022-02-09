<template id="to-do-item">
<li :class="{ done: done }" @click="$emit('done')" @mouseover="upHere = true" @mouseleave="upHere = false">  <!--
    v-for - řetězec pro vypsání obashu (item - prvek ve zdroji, items - zdroj dat) /* OPRAVENO */
    :key - každý výpis by měl mít unikátní "klíč", aby se nastalo že budou dva stejné výpisy - nastane error /* OPRAVENO */
    (používá se pro string a čísla, nepoužívat pro malé hodnoty jako obejkty a pole)
  -->
      <span>{{ content }}</span>
      <!--
        :class - pokud "todo.done" je aktivní, objeví se třída "done" /* NEOPRAVENO */
        @click - po kliknutí se ???
      -->
	<div v-show="upHere" class="icon icon-trash" @click="$emit('remove')"></div>
      <!--
        p
o kliknutí se odstraní todo ze seznamu listů
      -->
</li>
</template>

<script>
import { ref } from 'vue';
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'ToDoItem',
  props: {
    content: String,
    done: Boolean,
  },
  emits: ['remove', 'done'],
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
.done{
  background: $quaternalyCol;
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
