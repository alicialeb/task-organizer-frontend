<template>
  <v-app>
    <v-app-bar app dark>
      <div class="d-flex align-center">
        CleanTasks
      </div>

      <v-btn to="/tasks" text>
        Tasks
      </v-btn>
      <v-btn to="/add" text>
        Add Task
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',
};
</script>
