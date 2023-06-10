<script lang="ts" setup>
import { ref } from 'vue'
const data = localStorage.getItem('todolist')

const todolist = data !== null && data !== undefined ? JSON.parse(data) : []
const tasks = ref<Task[]>(todolist)

interface Task {
  name: string
  completed: boolean
  id: number
}

const newTaskName = ref('')

const addTask = () => {
  if (newTaskName.value !== '' && !tasks.value.some((x) => x.name === newTaskName.value)) {
    tasks.value.push({ name: newTaskName.value, completed: false, id: tasks.value.length })
    localStorage.setItem('todolist', JSON.stringify(tasks.value))
    newTaskName.value = ''
  }
}

const toggleTask = (idx: number) => {
  tasks.value[idx].completed = !tasks.value[idx].completed
}
</script>

<template>
  <div class="flex w-full gap-5">
    <section class="w-1/2 bg-base-200 p-5 rounded-box">
      <header class="font-bold badge badge-neutral text-neutral-content">未完</header>

      <ul>
        <li
          v-for="task in tasks.filter((x) => !x.completed)"
          :key="task.name"
          @click="toggleTask(task.id)"
          class="hover:text-primary transition-colors"
        >
          {{ task.name }}
        </li>
      </ul>
    </section>
    <section class="w-1/2 bg-base-200 p-5 rounded-box">
      <header class="font-bold badge badge-neutral text-neutral-content">完了</header>
      <ul>
        <li
          v-for="task in tasks.filter((x) => x.completed)"
          :key="task.name"
          @click="toggleTask(task.id)"
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
  <div className="join">
    <div>
      <div>
        <input
          type="text"
          v-model="newTaskName"
          placeholder="タスク名を入力"
          className="join-item input input-bordered "
        />
        <label className="label">
          <span v-if="tasks.some((x) => x.name === newTaskName)" className="label-text-alt "
            >同じタスク名はだめ！</span
          >
          <span v-if="newTaskName.length === 0" className="label-text-alt "
            >タスク名を書いて！</span
          >
        </label>
      </div>
    </div>

    <button @click="addTask" class="btn btn-primary join-item">add</button>
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
