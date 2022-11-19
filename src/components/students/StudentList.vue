<script>
import StudentService from "@/services/Students.service";
export default {
  name: "student-list",

  components: {
    StudentModal: () => import("@/components/students/StudentModal.vue"),
  },

  data: () => ({
    loading: false,
    header: [
      { text: "#ID", value: "id" },
      { text: "Nome", value: "name" },
      { text: "Idade", value: "age" },
      { text: "Gênero", value: "sex" },
      { text: "Ações", value: "actions", align: 'end' },
    ],
    students: [],

    genderList: {
      F: "Feminino",
      M: "Masculino",
    },
  }),

  created() {
    this.getStudents();
  },

  methods: {
    async getStudents() {
      this.loading = true;
      const result = await StudentService.select();
      this.students = result;
      this.loading = false;
    },

    async deleteItem(item) {
      await StudentService.remove(item.id);
      this.getStudents();
    },

    async editItem(item) {
      this.$refs.studentModal.edit(item);
    },

    async createStudent() {
      this.$refs.studentModal.createStudent();
    },
  },
};
</script>

<template>
  <v-container color="accent" class="mt-20">

    <v-card elevation="8" max-width="1000" color="accent" class="ma-auto">
      <v-card-title>
        <v-row>
          <v-col cols="3">
            <span>
              Estudantes
            </span>
          </v-col>
          <v-spacer></v-spacer>
          <v-col cols="1">
            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
                <v-btn fab color="primary" v-bind="attrs" v-on="on" @click="createStudent">
                  <v-icon dark>
                    mdi-plus
                  </v-icon>
                </v-btn>
              </template>
              <span>Adicionar estudante</span>
            </v-tooltip>

          </v-col>
        </v-row>
      </v-card-title>
      <v-data-table :headers="header" :items="students" hide-default-footer :loading="loading" loader-height="2"
        loading-text="Carregando dados...">
        <template v-slot:[`item.actions`]="{ item }">
          <v-tooltip bottom>
            <template v-slot:activator="{ on, attrs }">
              <v-icon @click="editItem(item)" color="blue" v-bind="attrs" v-on="on">
                mdi-pencil
              </v-icon>
            </template>
            <span>Alterar estudante</span>
          </v-tooltip>
          <v-tooltip bottom>
            <template v-slot:activator="{ on, attrs }">
              <v-icon @click="deleteItem(item)" color="red" v-bind="attrs" v-on="on">
                mdi-delete
              </v-icon>
            </template>
            <span>Excluir estudante</span>
          </v-tooltip>

        </template>
        <template v-slot:[`item.sex`]="{ item }">
          {{ genderList[item.sex] || 'não informado' }}
        </template>
        <template v-slot:[`item.age`]="{ item }">
          {{ item.age || 'não informado' }}
        </template>
        <template v-slot:no-data>
          <i>Sem dados para mostrar</i>
        </template>
      </v-data-table>
    </v-card>
    <StudentModal @success="getStudents" ref="studentModal" />
  </v-container>
</template>