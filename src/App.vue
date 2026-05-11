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
    <h1 style="font-weight: bold; letter-spacing: 1px; color: #F0EBD8">Tasks Tracker</h1>

    <div class="top-section">

      <form
        @submit.prevent="addTask"
        class="form-area"
      >
        <input
          v-model="name"
          required
          placeholder="Type your task here..."
          style="width: 480px; padding: 5px 7px; margin-bottom: 5px"
        >

        <button>
          Add Task
        </button>
      </form>

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
          <h2 style="color: #F0EBD8">
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
            >
              {{ task.taskName }}
            </span>

            <div class="controls">
              <input
                type="checkbox"
                v-model="task.done"
              >

              <button @click="removeTask(task)">
                Remove
              </button>
            </div>
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
.main {
  background-color: #0D1321;
  height: 100vh
}

.completed { 
  color: rgb(8, 107, 23); 
  text-decoration: line-through;
  text-decoration-color: rgb(21, 32, 38) 
} 

.pending { 
  color: rgb(153, 17, 17); 
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
  padding-right: 20px
}

.form-area {
  flex: 1;
  text-align: center
}

.task-stats {
  flex: 1;
  text-align: center;
  background-color: #1D2D44;
  border-radius: 10px;
  box-shadow: 2px 5px 10px #162132;
  padding: 5px
}

.task-display {
  text-align: center;
  background-color: #1D2D44;
  border-radius: 10px;
  box-shadow: 2px 5px 10px #162132;
  margin-top: 20px;
  margin-bottom: 20px;
  margin-left: 20px;
  margin-right: 20px;
  padding: 20px 20px;

}

.controls {
  justify-content: right;
  padding-right: 30px;
  display: flex;
  gap: 10px
}

span {
  flex: 1; 
  padding-top: 3px; 
  padding-bottom: 3px;
  padding-left: 30px;
  text-align: left
}

ol {
  padding-left: 0px
}

li {
  display: flex;
  align-items: center;
  background-color: #172436;
  padding-top: 2px;
  padding-bottom: 2px;
  margin-top: 5px;
  margin-bottom: 5px;
  margin-left: 30px;
  margin-right: 30px;
  border-radius: 5px
}

li:hover {
  background-color: #0D1321;
}

button {
  background-color: #748CAB;
  color: #F0EBD8;
  border-radius: 5px;
  border: none;
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 7px;
  padding-right: 7px
}

button:hover {
  background-color: #3E5C76;
}
</style>