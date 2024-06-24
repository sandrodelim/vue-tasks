<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue';

  // const tasks = reactive([
  //   { title: 'Task 1', done: false },
  //   { title: 'Task 2', done: true },
  //   { title: 'Task 3', done: false }
  // ]);
  let tasks = reactive([]);
  const title = ref('');

  function addTask() {
    if(title.value.trim() === '') return;
    if(tasks.some(task => task.title === title.value)) return;
    tasks.push({ title: title.value, done: false });
    console.log("Addtask: ", title.value);
    title.value = '';
  }
  function removeTask(item: any) {
    tasks.splice(tasks.indexOf(item), 1);
    console.log("RemoveTask: ", title.value);
  } 
  function toggleTask(item: any) {
    item.done = !item.done;
    console.log("ToggleTask: ", title.value);
  }

  onMounted(async () => {
    const result = await fetch('http://localhost:3000/task');
    tasks = await result.json();    
  });
</script>

<template>
  <div class="p-3">
    <h1 class="text-2xl p-2 text-center">VUE Tasks</h1>
    <hr>
    <input type="text" v-model="title" @keyup.enter="addTask()" class="p-2 my-4 text-slate-800 w-11/12"
      placeholder="Add Task" />
    <button @click="addTask()" class="p-2 bg-blue-500 text-white">Add</button>

    <div v-if="tasks.length === 0">
      <p class="text-center p-2">No tasks found</p>
    </div>

    <div v-for="item in tasks" class="">
      <div class="py-2">
        <span v-bind:style="{ 'text-decoration': (item.done) ? 'line-through' : '' }">{{ item.title }}</span>
        <button @click="toggleTask(item)" class="ml-2 text-white rounded-md bg-slate-600 py-1 px-2">{{ item.done ?
          'UnDone' : 'Done' }}</button>
        <button @click="removeTask(item)" class="p-2 ml-3 bg-yellow-800 text-white rounded-md">Remove</button>
      </div>
    </div>

  </div>
</template>

<style scoped>
</style>
