<template>
  <div>
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
  body{
    margin:0;
    padding:0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    background-color: black;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
