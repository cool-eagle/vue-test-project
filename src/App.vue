<template>
  <div :class="$style.app">
    <ToDo/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import ToDo from "./components/ToDo.vue";

export default defineComponent({
  name: 'App',
  components: {
    ToDo,
  },
  setup() {
    // Setup
  },
});
</script>

<style lang="scss" module>
.app{
    margin: 15vh 0 0 0;
  }
</style>
