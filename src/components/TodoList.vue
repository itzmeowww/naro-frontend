<script lang="ts" setup>
import { ref } from 'vue'

interface Task {
  name: string
  completed: boolean
  id: number
}

const tasks = ref<Task[]>([])
const newTaskName = ref('')

const addTask = () => {
  if (newTaskName.value !== '')
    tasks.value.push({ name: newTaskName.value, completed: false, id: tasks.value.length })
  newTaskName.value = ''
}

const toggleTask = (idx: number) => {
  tasks.value[idx].completed = !tasks.value[idx].completed
}
</script>

<template>
  <section class="">
    <header>完了済み</header>
    <ul>
      <li
        v-for="task in tasks.filter((x) => x.completed)"
        :key="task.name"
        @click="toggleTask(task.id)"
      >
        {{ task.name }}
      </li>
    </ul>
  </section>

  <section>
    <header>未完</header>
    <ul>
      <li
        v-for="task in tasks.filter((x) => !x.completed)"
        :key="task.name"
        @click="toggleTask(task.id)"
      >
        {{ task.name }}
      </li>
    </ul>
    <p>
      未完了のタスクの名前をクリックすると、そのタスクが完了します。
      完了したタスクの名前をクリックすると、そのタスクは未完了になります。
    </p>
  </section>

  <div>
    <label>
      名前
      <input v-model="newTaskName" type="text" />
    </label>

    <button @click="addTask">add</button>
  </div>
</template>

<style scoped>
section {
}
li:hover {
  text-decoration: line-through;
  cursor: pointer;
}
</style>
