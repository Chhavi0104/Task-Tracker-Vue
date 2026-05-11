<script setup>
import { ref } from 'vue'

const tasks = ref([])
const name = ref('')
const disp = ref(true)
const stats = ref(true)

function addTask() {
  const trimmed = name.value.trim()
  const duplicate = tasks.value.some(
    i => i.taskName === trimmed
  )

  if (trimmed && !duplicate) {
    tasks.value.push({
      taskName: trimmed,
      done: false
    })

    name.value = ''
  }

  else {
    name.value = ''
  }
}

function removeTask(name) {
  tasks.value = tasks.value.filter(
    (i) => i !== name
  )
}

function display() {
  disp.value = !disp.value
}

function taskStats() {
  stats.value = !stats.value
}
</script>

<template>
  <div class="main">
    <h1>Tasks Tracker</h1>

    <div class="top-section">

      <form
        @submit.prevent="addTask"
        class="form-area"
      >
        <input
          v-model="name"
          required
          placeholder="Type your task here..."
        >

        <button>
          Add Task
        </button>
      </form>
      
      <div class="divider"></div>

      <div class="task-stats">

        <button
          v-if="stats"
          @click="taskStats()"
        >
          Hide Task Statistics
        </button>

        <button
          v-else
          @click="taskStats()"
        >
          Display Task Statistics
        </button>

        <div v-if="stats">
          <h2>
            Total Tasks: {{ tasks.length }}
          </h2>

          <h3>
            Completed Tasks:
            {{ tasks.filter((i) => i.done === true).length }}
          </h3>

          <h3>
            Incomplete Tasks:
            {{ tasks.filter((i) => i.done === false).length }}
          </h3>
        </div>

      </div>
    </div>

    <div class="task-display">

      <button
        v-if="disp"
        @click="display()"
      >
        Hide Tasks
      </button>

      <button
        v-else
        @click="display()"
      >
        Display Tasks
      </button>

      <div v-if="disp">
        <ol>
          <li
            v-for="task in tasks"
            :key="task.taskName"
          >
            <span
              :class="{
                completed: task.done,
                pending: !task.done
              }"
              style="flex: 1"
            >
              {{ task.taskName }}
            </span>

            <input
              type="checkbox"
              v-model="task.done"
            >

            <button @click="removeTask(task)">
              Remove
            </button>
          </li>
        </ol>

        <p v-if="tasks.length === 0">
          No tasks to display
        </p>
      </div>

    </div>
  </div>
</template>

<style> 
.completed { 
  color: green; 
  text-decoration: line-through; 
} 

.pending { 
  color: red; 
} 

button { 
  margin-left: 10px; 
  margin-right: 10px; 
  margin-top: 5px; 
  margin-bottom: 5px 
} 

.top-section {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  margin-top: 10px;
  margin-bottom: 10px;
  padding-left: 20px;
  padding-right: 20px;
  border-top: solid medium antiquewhite;
  border-bottom: solid medium antiquewhite;
}

.divider {
  width: 1px;
  background-color:antiquewhite;
  height: 200px
}

.form-area {
  flex: 1;
  text-align: center
}

.task-stats {
  flex: 1;
  text-align: center
}

li {
  display: flex;
  align-items: center;
  list-style-type: style type lower-roman greek;
}
</style>