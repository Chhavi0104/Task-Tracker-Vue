<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const name = ref('')
const disp = ref(true)
const stats = ref(true)
const totalTasks = computed(() => {return tasks.value.length})
const completedTasks = computed(() => {return tasks.value.filter((i) => i.done === true).length})
const incompleteTasks = computed(() => {return tasks.value.filter((i) => i.done === false).length})
const currentFilter = ref('all')
const filteredTasks = computed(() => {
  if (currentFilter.value === 'all') {
    return tasks.value
  }

  else if (currentFilter.value === 'completed') {
    return tasks.value.filter(
      (i) => i.done === true
    )
  }

  else if (currentFilter.value === 'incomplete') {
    return tasks.value.filter(
      (i) => i.done === false
    )
  }
})

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
          class="userInput"
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
          <h2>
            Total Tasks: {{ totalTasks}}
          </h2>

          <h3>
            Completed Tasks:
            {{ completedTasks }}
          </h3>

          <h3>
            Incomplete Tasks:
            {{ incompleteTasks }}
          </h3>
        </div>

      </div>
    </div>

    <div class="task-display">
      <div class="task-controls">
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

        <button
          @click="currentFilter='all'"
          :class="{activeFilter: currentFilter==='all', button}" 
        >
          All Tasks
        </button>

        <button
          @click="currentFilter='completed'"
          :class="{activeFilter: currentFilter==='completed', button}"
        >
          Completed Tasks
        </button>

        <button
          @click="currentFilter='incomplete'"
          :class="{activeFilter: currentFilter==='incomplete', button}"
        >
          Incomplete Tasks
        </button>
      </div>
      <div v-if="disp">
        <ol>
          <li
            v-for="task in filteredTasks"
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
  margin: 5px 10px; 
} 

.top-section {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  margin: 10px 0px;
  padding: 0px 20px;
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
  margin: 20px;
  padding: 20px 20px;

}

.controls {
  justify-content: flex-end;
  padding-right: 30px;
  display: flex;
  gap: 10px
}

.userInput {
  width: 480px; 
  padding: 5px 7px; 
  margin-bottom: 5px
}

.activeFilter {
  background-color: #3E5C76;
}

span {
  flex: 1; 
  padding: 3px 0px 3px 30px; 
  text-align: left
}

ol {
  padding-left: 0px
}

li {
  display: flex;
  align-items: center;
  background-color: #172436;
  padding: 2px 0px;
  margin: 5px 30px;
  border-radius: 5px
}

li:hover {
  background-color: #0D1321;
  transition: 5ms ease;
  transform: scale(1.005)
}

button {
  background-color: #748CAB;
  color: #F0EBD8;
  border-radius: 5px;
  border: none;
  padding: 5px 7px;
}

button:hover {
  background-color: #3E5C76;
  transition: 5ms ease;
  transform: scale(1.01)
}

h1 {
  font-weight: bold; 
  letter-spacing: 1px; 
  color: #F0EBD8
}

h2 {
  color: #F0EBD8
}
</style>