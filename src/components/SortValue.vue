<template>
    <button @click="$emit('sortValueClick')">Sort by Value</button>
</template>

<script lang="ts">
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'SortValue',
  emits: ['sortValueClick'],
})
</script>

<style lang="scss">

</style>
