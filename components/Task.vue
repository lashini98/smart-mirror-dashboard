<template>
  <div class="task">
    <div class="task__name">
      <span>{{ task.name }}</span>
      <span>{{ task.date }}</span>
    </div>
    <div class="task__share">
      <button @click="sendEmail" class="my-2 my-sm-0 w-100 delete-task__btn">
        <img src="/images/share.png" alt="share" />
      </button>
    </div>
    <div class="task__done">
      <button
        @click="$emit('editTask', task.id)"
        class="my-2 my-sm-0 w-100 edit-task__btn"
      >
        <img src="/images/edit.png" alt="edit" />
      </button>
    </div>
    <div class="task__remove">
      <button
        @click="$emit('deleteTask', task.id)"
        size="sm"
        class="my-2 my-sm-0 w-100 delete-task__btn"
      >
        <img src="/images/remove.png" alt="remove" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: {
      type: Object,
      required: true,
    },
  },

  methods: {
    sendEmail(task) {
      Email.send({
        Host: "smtp.gmail.com",
        Username: "cubcaremux@gmail.com",
        Password: "cubcare123",
        To: "lashini.hk@gmail.com",
        From: "cubcaremux@gmail.com",
        Subject: "Current task",
        Body: task,
      }).then((message) => alert("Email sent successfully"));
    },
  },
};
</script>

<style lang="scss">
.task {
  display: grid;
  grid-template-columns: 1fr 45px 45px 45px;
  margin-bottom: 5px;
  grid-column-gap: 5px;
  padding: 5px 15px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  align-items: center;
  border-radius: 5px;
  font-weight: 500;

  &__name {
    display: flex;
    flex-direction: column;
    font-size: 17px;

    span:last-child {
      font-weight: 400;
      color: #c5c5c5;
      font-size: 13px;
    }
  }
  button {
    outline: 0;
    border: none;
    background-color: transparent;
    img {
      width: 30px;
      cursor: pointer;
    }
  }
}
</style>
