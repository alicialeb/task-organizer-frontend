<template>
  <div class="submit-form mt-3 mx-auto">
    <p class="headline">Add Task</p>

    <div v-if="!saved">
      <v-form ref="form" lazy-validation>
        <v-text-field
          v-model="task.title"
          :rules="[(v) => !!v || 'Title is required']"
          label="Title"
          required
        ></v-text-field>

        <v-text-field
            v-model="task.description"
            :rules="[(v) => !!v || 'Description is required']"
            label="Description"
            required
        ></v-text-field>
      </v-form>

      <v-btn color="primary" class="mt-3" @click="saveTask">Save</v-btn>
    </div>

    <div v-else>
      <v-card class="mx-auto">
        <v-card-title>
          Saved successfully!
        </v-card-title>

        <v-card-subtitle>
          Click the button to add another task.
        </v-card-subtitle>

        <v-card-actions>
          <v-btn color="success" @click="newTask">Add</v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </div>
</template>

<script>
import TaskDataService from "../services/TaskDataService";

export default {
  name: "add-task",
  data() {
    return {
      task: {
        id: null,
        title: "",
        description: "",
        status: true,
      },
      saved: false,
    };
  },

  methods: {
    saveTask() {
      const data = {
        title: this.task.title,
        description: this.task.description
      };

      TaskDataService.create(data)
        .then((response) => {
          this.task.id = response.data.id;
          console.log(response.data);
          this.saved = true;
        })
        .catch((e) => {
          console.log(e);
        });
    },

    newTask() {
      this.saved = false;
      this.task = {};
    }
  }
};
</script>

<style lang="scss" scoped>
.submit-form {
  max-width: 300px;
}
</style>