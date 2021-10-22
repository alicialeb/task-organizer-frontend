<template>
  <v-row align="center" class="list px-3 mx-auto">
    <v-col cols="12" md="8">
      <v-text-field v-model="title" label="Search by title"></v-text-field>
    </v-col>

    <v-col cols="12" md="4">
      <v-btn small @click="searchTitle">
        Search
      </v-btn>
    </v-col>

    <v-col cols="12" sm="12">
      <v-card class="mx-auto" title>
        <v-card-title>Tasks</v-card-title>

        <v-data-table
            :headers="headers"
            :items="tasks"
            disable-pagination
            :hide-default-footer="true"
        >
          <template v-slot:[`item.actions`]="{ item }">
            <v-icon small class="mr-2" @click="editTask(item.id)">mdi-pencil</v-icon>
            <v-icon small @click="deleteTask(item.id)">mdi-delete</v-icon>
          </template>
        </v-data-table>

        <v-card-actions v-if="tasks.length > 0">
          <v-btn small color="error" @click="removeAllTasks">Remove All</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import TaskDataService from "../services/TaskDataService";

export default {
  name: "tasks-list",
  data() {
    return {
      tasks: [],
      title: "",
      headers: [
        {text: "Title", align: "start", sortable: false, value: "title"},
        {text: "Description", value: "description", sortable: false},
        {text: "Status", value: "status", sortable: false},
        {text: "Actions", value: "actions", sortable: false}
      ],
    };
  },

  methods: {
    retrieveTasks() {
      TaskDataService.getAll()
          .then((response) => {
            this.tasks = response.data.map(this.getDisplayTask);
            console.log(response.data);
          })
          .catch((e) => {
            console.log(e);
          });
    },

    refreshList() {
      this.retrieveTasks();
    },

    removeAllTasks() {
      TaskDataService.deleteAll()
          .then((response) => {
            console.log(response.data);
            this.refreshList()
          })
          .catch((e) => {
            console.log(e);
          });
    },

    searchTitle() {
      TaskDataService.findByTitle(this.title)
          .then((response) => {
            this.tasks = response.data.map(this.getDisplayTask);
            console.log(response.data);
          })
          .catch((e) => {
            console.log(e);
          });
    },

    editTask(id) {
      this.$router.push({ name: "task-details", params: {id: id}});
    },

    deleteTask(id) {
      TaskDataService.delete(id)
          .then(() => {
            this.refreshList();
          })
          .catch((e) => {
            console.log(e);
          });
    },

    getDisplayTask(task) {
      return {
        id: task.id,
        title: task.title.length > 30 ? task.title.substr(0, 30) + "..." :task.title,
        description: task.description.length > 30 ? task.description.substr(0, 30) + "..." :task.description,
        status: task.status ? "Done" : "Open"
      };
    }
  },

  mounted() {
    this.retrieveTasks();
  }
};
</script>

<style lang="scss" scoped>
.list {
  max-width: 750px;
}
</style>