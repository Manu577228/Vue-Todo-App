<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Bharadwaj Vue Todo App</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";

const task = ref("");
let editedTask = ref(null);
const statuses = ref(["to-do", "in-progress", "finished"]);

/* Status could be: 'to-do' / 'in-progress' / 'finished' */
const tasks = ref([
  {
    name: "Learn Vue.js Today with TodoApp.",
    status: "in-progress",
  },
  {
    name: "Meditate.",
    status: "to-do",
  },
  {
    name: "Create YouTube video.",
    status: "finished",
  },
]);

/**
 * Capitalize first character
 */
function capitalizeFirstChar(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

/**
 * Change status of task by index
 */
function changeStatus(index) {
  let newIndex = statuses.value.indexOf(tasks.value[index].status);
  if (++newIndex > 2) newIndex = 0;
  tasks.value[index].status = statuses.value[newIndex];
}

/**
 * Deletes task by index
 */
function deleteTask(index) {
  tasks.value.splice(index, 1);
}

/**
 * Edit task
 */
function editTask(index) {
  task.value = tasks.value[index].name;
  editedTask.value = index;
}

/**
 * Add / Update task
 */
function submitTask() {
  if (task.value.length === 0) return;

  /* We need to update the task */
  if (editedTask.value !== null) {
    tasks.value[editedTask.value].name = task.value;
    editedTask.value = null;
  } else {
    /* We need to add a new task */
    tasks.value.push({
      name: task.value,
      status: "to-do",
    });
  }

  task.value = "";
}
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                              supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>
