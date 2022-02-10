<template>
    <button @click="$emit('sortDateClick')">Sort by Added Date</button>
</template>

<script lang="ts">
import { defineComponent } from '@vue/runtime-core';

export default defineComponent({
  name: 'SortDate',
  emits: ['sortDateClick'],
})
</script>

<style lang="scss">

</style>
