<script setup lang="ts">
import { ref, computed } from 'vue'

const count = ref<number>(0)
const countMessage = computed(() => '回数: ' + count.value)
</script>

<template>
  <div>
    <div class="text-center text-xl my-4">{{ countMessage }}</div>
    <div class="join">
      <button @click="count++" class="join-item btn btn-outline btn-primary">クリック！</button>
      <button @click="count = 0" class="join-item btn btn-outline btn-primary">リセット！</button>
    </div>
  </div>
</template>

<style></style>
