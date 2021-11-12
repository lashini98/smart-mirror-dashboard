<template>
  <div class="task-app py-3">
    <b-col md="8" offset-md="2">
      <h3 class="text-center">Task List</h3>
      <b-form-input
        v-model="searchTerm"
        class="mr-sm-2 w-100"
        placeholder="Search"
        @keydown="search"
      ></b-form-input>
      <AddTask @addTask="addTask"/>
      <TaskList
        :taskList="searchTerm.length ? searchResults : tasks"
        @editTask="openUpdateTaskModal"
        @deleteTask="deleteTask"/>
    </b-col>
    <b-modal
      hide-header-close
      title="Edit Task"
      v-model="editTaskModal"
      hide-footer
      content-class="edit-modal"
    >
      <template #modal-title>
        Edit Task
        <img
          class="edit-modal__close"
          src="/images/close.png"
          alt="close"
          @click="editTaskModal = false">
      </template>
      <div class="d-block" v-if="currentTask">
        <b-form @submit.prevent="updateTask">
          <b-col md="10" offset-md="1">
            <b-row>
              <b-col md="12" class="mb-2">
                <b-form-input
                  v-model="currentTask.name"
                  class="w-100"
                  placeholder="Add task"
                ></b-form-input>
              </b-col>
              <b-col md="12" class="mb-2">
                <date-picker
                  v-model="currentTask.date"
                  class="w-100"
                  valueType="format"></date-picker>
              </b-col>
              <b-col md="8" offset-md="2" class="mt-3">
                <b-button
                  variant="info"
                  class="my-2 my-sm-0 w-100 add-task__btn"
                  type="submit"
                >Update Task
                </b-button>
              </b-col>
            </b-row>
          </b-col>
        </b-form>
      </div>
    </b-modal>
  </div>
</template>

<script>
import AddTask from "../components/AddTask";
import TaskList from "../components/TaskList";
import {v4 as uuidv4} from "uuid";
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';

export default {
  components: {
    AddTask,
    TaskList,
    DatePicker
  },

  data: () => ({
    tasks: [],
    editTaskModal: false,
    currentTask: null,
    searchTerm: '',
    searchResults: []
  }),
  mounted() {
    const localStorageTasks = this.getTasksFromLocalStorage();
    this.tasks = localStorageTasks ? localStorageTasks : [];
  },
  async created() {
    await fetch('https://api.openweathermap.org/data/2.5/weather?q=new%20york&units=standard&appid=39a9a737b07b4b703e3d1cd1e231eedc')
      .then(response => response.json())
      .then(data => console.log(data));
  },
  methods: {
    addTask(task) {
      const newTask = {...task};
      newTask.id = uuidv4();
      this.tasks.push(newTask);

      this.addTasksToLocalStorage();
    },
    deleteTask(taskId) {
      const index = this.tasks.findIndex((task) => task.id === taskId);
      this.tasks.splice(index, 1);

      this.addTasksToLocalStorage();
    },
    updateTask() {
      const newTasks = [...this.tasks];
      const taskIndex = newTasks.findIndex((task) => task.id === this.currentTask.id);
      newTasks[taskIndex] = {...this.currentTask};
      this.tasks = newTasks;
      this.editTaskModal = false;
      this.currentTask = null;

      this.addTasksToLocalStorage();
    },
    openUpdateTaskModal(taskId) {
      const task = this.tasks.find((task) => task.id === taskId);
      this.currentTask = {...task};
      this.editTaskModal = true;
    },
    addTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    getTasksFromLocalStorage() {
      const tasks = localStorage.getItem('tasks');
      return JSON.parse(tasks);
    },
    search() {
      if (!this.searchTerm) return;
      this.searchResults = this.tasks.filter(task => task.name.toLowerCase().includes(this.searchTerm.toLowerCase()));
    }
  },
};
</script>
