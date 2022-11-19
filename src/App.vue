<script>
import StudentList from "@/components/students/StudentList.vue";

export default {
  name: "App",

  components: {
    StudentList,
  },

  data: () => ({
    //
  }),
};
</script>

<template>
  <v-app>
    <v-app-bar app elevation="4" color="deep-purple accent">
      <v-toolbar-title>Voss School</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-title>Aplicação para estudo</v-toolbar-title>

    </v-app-bar>
    <v-main>
      <v-container fluid>
        <StudentList />
      </v-container>
    </v-main>
    <v-footer app color="deep-purple accent">
      TopDev® - 2022
    </v-footer>
  </v-app>
</template>