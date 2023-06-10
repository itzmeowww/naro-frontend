<script lang="ts" setup>
import { ref } from 'vue'
const data = localStorage.getItem('todolist')

const todolist = data !== null && data !== undefined ? JSON.parse(data) : []
const tasks = ref<Task[]>(todolist)

interface Task {
  name: string
  completed: boolean
}

const newTaskName = ref('')

let empty = ref(false)

const addTask = () => {
  if (newTaskName.value !== '' && !tasks.value.some((x) => x.name === newTaskName.value)) {
    tasks.value.push({ name: newTaskName.value, completed: false })
    localStorage.setItem('todolist', JSON.stringify(tasks.value))
    newTaskName.value = ''
  }
}

const toggleTask = (name: string) => {
  const idx = tasks.value.findIndex((x) => {
    return x.name === name
  })
  if (idx >= 0) {
    tasks.value[idx].completed = !tasks.value[idx].completed
  }
}
const handleOnChange = () => {
  console.log('jo')
  if (newTaskName.value.length === 0) {
    empty.value = true
  } else {
    empty.value = false
  }
}

const handleOnSubmit = (e: Event) => {
  e.preventDefault()
}
</script>

<template>
  <div class="flex flex-col sm:flex-row w-full gap-5">
    <section class="w-full sm:w-1/2 bg-base-200 p-5 rounded-box">
      <header class="font-bold badge badge-neutral text-neutral-content">未完</header>

      <ul>
        <li
          v-for="task in tasks.filter((x) => !x.completed)"
          :key="task.name"
          @click="toggleTask(task.name)"
          class="hover:text-primary transition-colors"
        >
          {{ task.name }}
        </li>
      </ul>
    </section>
    <section class="w-full sm:w-1/2 bg-base-200 p-5 rounded-box">
      <header class="font-bold badge badge-neutral text-neutral-content">完了</header>
      <ul>
        <li
          v-for="task in tasks.filter((x) => x.completed)"
          :key="task.name"
          @click="toggleTask(task.name)"
          class="hover:text-primary transition-colors"
        >
          {{ task.name }}
        </li>
      </ul>
    </section>
  </div>
  <div class="text-secondary text-xs">
    未完了のタスクの名前をクリックすると、そのタスクが完了します。
    なお、完了したタスクの名前をクリックすると、そのタスクは未完了になります。
  </div>
  <form class="join" @submit="handleOnSubmit">
    <div>
      <div>
        <input
          type="text"
          v-model="newTaskName"
          placeholder="タスク名を入力"
          class="join-item input input-bordered"
          :class="{
            'border-error': tasks.some((x) => x.name === newTaskName) || empty
          }"
          @input="handleOnChange"
        />
        <label className="label">
          <span v-if="tasks.some((x) => x.name === newTaskName)" class="label-text-alt text-red-400"
            >同じタスク名はだめ！</span
          >
          <span v-if="empty" className="label-text-alt text-error">タスク名を書いて！</span>
        </label>
      </div>
    </div>

    <button @click="addTask" class="btn btn-primary join-item">add</button>
  </form>
</template>

<style scoped>
section {
}
li:hover {
  text-decoration: line-through;
  cursor: pointer;
}
</style>
