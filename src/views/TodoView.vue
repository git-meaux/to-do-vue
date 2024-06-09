<script setup>
import { ref, watch, computed } from 'vue';
import { uid } from 'uid';
import { Icon } from '@iconify/vue';
import TaskCreator from '@/components/TaskCreator.vue';
import TaskItem from '@/components/TaskItem.vue';

const taskList = ref([]);

watch(
  taskList, 
  () => {SetLocalStorage()},
  {deep: true}
);

const tasksCompleted = computed(
  () => taskList.value.every( (task) => task.isCompleted )
);

const SetLocalStorage = () => {
  localStorage.setItem('taskList',JSON.stringify(taskList.value));
}
const GetLocalStorage = () => {
  const savedTaskList = JSON.parse(localStorage.getItem('taskList'));
  if (savedTaskList !== null) {
    taskList.value = savedTaskList;
  }
}
GetLocalStorage();

const createTask = (task) => {
  taskList.value.push({
    id: uid(),
    task,
    isCompleted: null,
    isEditing: null,
  });
};

const toggleTaskComplete = (key) => {
  taskList.value[key].isCompleted = !taskList.value[key].isCompleted;
}
const taskToggleEdit = (key) => {
  taskList.value[key].isEditing = !taskList.value[key].isEditing;
}
const updateTask = (value, key) => {
  taskList.value[key].task = value;
}
const deleteTask = (id) => {
  taskList.value = taskList.value.filter( (task) => task.id !== id );
}
</script>

<template>
  <main>
    <h1>
      Create Task:
    </h1>
    <TaskCreator @create-task="createTask" />
    <ul v-if="taskList.length > 0" class="task-list">
      <TaskItem v-for="(item, index) in taskList" 
      :task="item" 
      :index="index" 
      @toggle-complete="toggleTaskComplete" 
      @edit-task="taskToggleEdit"
      @update-task="updateTask"
      @delete-task="deleteTask"
      />
    </ul>
    <p v-else class="task-msg">
      <Icon icon="streamline-emojis:weary-face-2" width="22" height="22"></Icon>
      <span>You have no items to complete. Add one.</span>
    </p>
    <h2 v-if="tasksCompleted && taskList.length > 0" class="task-msg ">All tasks completed!</h2>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .task-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .task-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>