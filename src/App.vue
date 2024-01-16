<script setup>
import { ref, onMounted } from 'vue';

const tasks = ref([
  { name: 'First Task To Complete An Assignment Within 10 Second.', time: 10, timeLeft: 10 },
  { name: 'Second Task To Complete An Assignment Within 10 Second.', time: 30, timeLeft: 30 },
]);

const showAddTaskPopup = ref(false);
const newTaskName = ref('');
const newTaskTime = ref(0);
const activeTaskIndex = ref(0);

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t !== task);
};

onMounted(() => {
  const intervalId = setInterval(() => {
    if (tasks.value[activeTaskIndex.value]) {
      const updatedTasks = tasks.value.map((task, index) => {
        if (index === activeTaskIndex.value) {
          return {
            ...task,
            timeLeft: task.timeLeft > 0 ? task.timeLeft - 1 : 0
          };
        } else {
          return task;
        }
      });
      tasks.value = updatedTasks;
      if (updatedTasks[activeTaskIndex.value].timeLeft === 0) {
        activeTaskIndex.value++;
      }
    } else {
      clearInterval(intervalId);
    }
  }, 1000);
});

const addTask = () => {
  tasks.value.push({ name: newTaskName.value, time: parseInt(newTaskTime.value), timeLeft: parseInt(newTaskTime.value) });
  showAddTaskPopup.value = false;
  newTaskName.value = '';
  newTaskTime.value = 0;
};
</script>

<template>
    <h3>Here we Provide the time and with countdown timer. <br> You Have to remove your task within providing time each task. (Here Is Time) (Here is Counter)</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="index">
      <h4 style="display: flex;"><span style="padding-right: 20px;;">{{ index+1 }} .</span>  {{  task.name  }}</h4>
      ({{ task.time }} seconds) ({{ task.timeLeft }} seconds)
      <button :disabled="task.timeLeft == 0 || index != activeTaskIndex" @click="removeTask(task)">Remove</button>
    </li>
  </ul>
  <button @click="showAddTaskPopup = true">Add Task</button>

  <div v-if="showAddTaskPopup" class="modal">
    
    <form @submit.prevent="addTask">
        <h4 style="color: black;">Add Task Here with Provide Time</h4>
      <input type="text" v-model="newTaskName" class="form-control" placeholder="Task Name">
      <input type="number" v-model="newTaskTime" class="form-control" placeholder="Time (seconds)">
      <button type="submit">Add</button>
      <button @click="showAddTaskPopup = false" type="submit">Close</button>
    </form>
  </div>
</template>
<style scoped>
.modal{
    position: fixed;
    height: 100vh;
    width: 100%;
    border-radius: 10px;
    top: 0;
    left: 0;
    display: grid;
    place-content: center;
}
.modal form{
    height: auto;
    width: 400px;
    border-radius: 10px;
    border: 1px solid #202020;
    background: #a8a7a7;
    padding: 20px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
}
.form-control{
    padding: 10px;
    border-radius:10px;
    width: 400px;
}
li{
    list-style: none;
}
</style>
