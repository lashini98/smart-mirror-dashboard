<template>
  <div class="add-task mt-3">
    <b-form @submit.prevent="addTask">
      <b-row class="add-task__wrap">
        <b-col md="7" class="position-relative">
          <b-form-input
              v-model="task.name"
              class="mr-sm-2 w-100"
              placeholder="Add new task"
          ></b-form-input>
        </b-col>
        <b-col md="3">
          <date-picker
              class="w-100"
              v-model="task.date"
              valueType="format"></date-picker>
        </b-col>
        <b-col md="2">
          <b-button
              variant="info"
              class="my-2 my-sm-0 w-100 add-task__btn"
              type="submit"
          >Add Task
          </b-button
          >
        </b-col>
      </b-row>
    </b-form>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import moment from "moment";

export default {
  name: "AddTask",
  components: {DatePicker},
  data: () => ({
    task: {
      name: '',
      date: ''
    },
  }),
  created() {
    this.setToday();
  },
  methods: {
    addTask() {
      if (this.task.name) {
        this.$emit("addTask", this.task);
        this.task.name = "";
        this.setToday();
      }
    },
    setToday() {
      this.task.date = moment().format('YYYY-MM-DD');
    }
  },
};
</script>

<style lang="scss">
.add-task {
  &__calendar {
    position: absolute;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    cursor: pointer;
  }
}
</style>
