<script setup lang="ts">
import ClickCounter from './ClickCounter.vue'
import ItemList from './ItemList.vue'
import TodoList from './TodoList.vue'

defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="w-screen gap-3 flex flex-col justify-center items-center mx-auto max-w-4xl p-5 py-12">
    <ClickCounter />
    <ItemList />
    <TodoList />
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
