<template>
  <div v-if="currentTask" class="edit-form py-3">
    <p class="headline">Edit Task</p>

    <v-form ref="form" lazy-validation>
      <v-text-field
        v-model="currentTask.title"
        :rules="[(v) => !!v || 'Title is required']"
        lable="Title"
        required
      ></v-text-field>

      <v-text-field
          v-model="currentTask.description"
          :rules="[(v) => !!v || 'Description is required']"
          lable="Description"
          required
      ></v-text-field>

      <label><strong>Status:</strong></label>
      {{currentTask.status ? "Done" : "Open"}}

      <v-divider class="my-5"></v-divider>

      <v-btn v-if="currentTask.status" @click="updateStatus(false)" color="primary small class=mr-2">ReOpen</v-btn>

      <v-btn v-if="currentTask.status" @click="updateStatus(true)" color="primary small class=mr-2">Done</v-btn>

      <v-btn color="error" small class="mr-2" @click="deleteTask">Delete</v-btn>

      <v-btn color="success" small @click="updateTask">Update</v-btn>
    </v-form>

    <p class="mt-3">{{message}}</p>
  </div>

  <div v-else>
    <p Please click on a task.></p>
  </div>
</template>

<script>
import TaskDataService from "../services/TaskDataService";

export default {
  name: "Task",
  data() {
    return {
      currentTask: null,
      message: "",
    };
  },

  methods: {
    getTask(id) {
      TaskDataService.get(id)
          .then((response) => {
            this.currentTask = response.data;
            console.log(response.data);
      })
      .catch((e) => {
        console.log(e);
      });
    },

    updateStatus(status) {
      const data = {
        id: this.currentTask.id,
        title: this.currentTask.title,
        description: this.currentTask.description,
        status: this.currentTask.status
      };

      TaskDataService.update(this.currentTask.id, data)
        .then((response) => {
        this.currentTask.status = status;
        console.log(response.data);
      })
      .catch((e) => {
        console.log(e);
      });
    },

    updateTask() {
      TaskDataService.update(this.currentTask.id, this.currentTask)
        .then((response) => {
          console.log(response.data);
          this.message = "The task has been updated successfully!";
        })
        .catch((e) => {
          console.log(e);
        });
    },

    deleteTask() {
      TaskDataService.delete(this.currentTask.id)
        .then((response) => {
          console.log(response.data);
          this.$router.push({ name: "tasks" });
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },

  mounted() {
    this.message = "";
    this.getTask(this.$route.params.id);
  },
};
</script>

<style lang="scss" scoped>
.edit-form {
  max-wdith: 300px;
  margin:auto;
}
</style>