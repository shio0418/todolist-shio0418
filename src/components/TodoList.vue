<script setup lang="ts">
import { ref, computed } from 'vue'

interface Task {
    name: string
    isFinished: boolean
}

const tasks = ref<Task[]>([
    { name: '宿題', isFinished: false },
    { name: '水やり', isFinished: true }
])

const finishedTasks = computed(() => tasks.value.filter((task) => task.isFinished))
const notFinishedTasks = computed(() => tasks.value.filter((task) => !task.isFinished))

const newTaskName = ref('')

const addTask = () => {
    const name = newTaskName.value
    if (!name) {
        alert("タスク名を入力してください")
        return
    }

    tasks.value.push({name: name, isFinished: false})
    newTaskName.value = ''
}

const finishTask = (taskName: string) => {
    const task = tasks.value.find((task) => (taskName === task.name))
    if (task) {
        task.isFinished = true
    }
}


</script>

<template>
    <div>TodoList</div>
    <div>完了リスト</div>
    <ul>
        <li v-for="task in finishedTasks" :key="task.name">
            <div>タスク: {{ task.name }}</div>
        </li>
    </ul>
    <div>未完了リスト</div>
    <ul>
        <li v-for="task in notFinishedTasks" :key="task.name">
            <div>タスク: {{ task.name }}</div>
            <div>
                <button @click="finishTask(task.name)">完了する</button>
            </div>
        </li>
    </ul>
    <div>
        <label>
            タスク名
            <input v-model="newTaskName" type="text" />
        </label>
        <button @click="addTask">追加</button>
    </div>
</template>