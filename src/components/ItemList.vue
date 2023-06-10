<script setup lang="ts">
import { ref } from 'vue'

interface Item {
  name: string
  price: number
}

const items = ref<Item[]>([
  { name: 'たまご', price: 100 },
  { name: 'りんご', price: 160 }
])
const newItemName = ref('')
const newItemPrice = ref(0)

const addItem = () => {
  if (newItemName.value !== '')
    items.value.push({ name: newItemName.value, price: newItemPrice.value })
  newItemName.value = ''
}
</script>

<template>
  <div class="bg-base-200 p-5 rounded-box">
    <header class="font-bold badge badge-neutral text-neutral-content">Item List</header>
    <ul class="flex flex-col gap-2 my-3">
      <li v-for="item in items" :key="item.name" :class="{ over500: item.price >= 500 }" class="">
        <div class="join">
          <div class="join-item bg-base-100 text-base-content px-2">
            {{ item.name }}
          </div>
          <div class="join-item bg-base-100 text-base-content px-2">{{ item.price }} 円</div>
          <div v-if="item.price >= 10000" class="join-item bg-warning text-warning-content px-2">
            高額商品
          </div>
        </div>
      </li>
    </ul>
    <div class="flex gap-5">
      <label>
        <input v-model="newItemName" type="text" class="input" placeholder="名前" />
      </label>
      <label>
        <input v-model="newItemPrice" type="number" class="input" placeholder="価格" />
      </label>
      <button @click="addItem" class="btn btn-outline">add</button>
    </div>
  </div>
</template>

<style>
.over500 {
  color: red;
}
</style>
