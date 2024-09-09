<template>
  <div>
    <div class="text-center">
      <v-btn
        color="success"
        @click="insertDialog=true"
      >
        <v-icon>fas fa-plus-circle</v-icon> เพิ่ม
      </v-btn>
    </div>
    <v-data-table
      :headers="headers"
      :items="teachers"
    >
      <template v-slot:item.index="{item}">
        {{ teachers.indexOf(item)+1 }}
      </template>
      <template v-slot:item.Username="{item}">
        {{ item.Username }}
        <v-chip small color="green" v-if="item.isAdmin==1">
          Admin
        </v-chip>
      </template>
      <template v-slot:item.actions="{item}">
        <v-btn text color="warning" small @click="edit(item)">
          <v-icon small>mdi-pencil</v-icon>
        </v-btn>
        <v-btn text color="error" small @click="remove(item)">
          <v-icon small>mdi-delete</v-icon>
        </v-btn>
      </template>
    </v-data-table>

    <v-dialog
      v-model="insertDialog"
      width="800"
      persistent
    >
      <div class="mb-2 text-right">
        <v-btn
          @click="insertDialog=false"
          small
          outlined
        >
          <v-icon small>fas fa-times-circle</v-icon> ปิด
        </v-btn>
      </div>
      <TeacherInsertForm @insertStatus="insertStatus" />
    </v-dialog>

    <v-dialog
      v-model="updateDialog"
      width="800"
      persistent
    >
      <div class="mb-2 text-right">
        <v-btn
          @click="updateDialog=false"
          small
          outlined
        >
          <v-icon small>fas fa-times-circle</v-icon> ปิด
        </v-btn>
      </div>
      <TeacherUpdateForm :teacherdata="teacher" @insertStatus="insertStatus" />
    </v-dialog>

    <v-dialog
      v-model="deleteDialog"
      width="800"
      persistent
    >
      <div class="mb-2 text-right">
        <v-btn
          @click="deleteDialog=false"
          small
          outlined
        >
          <v-icon small>fas fa-times-circle</v-icon> ปิด
        </v-btn>
      </div>
      <TeacherDeleteForm :teacherdata="teacher" @insertStatus="insertStatus" />
    </v-dialog>

  </div>
</template>

<script>
export default {
  data() {
    return {
      teachers: [],
      teacher: {},
      insertDialog: false,
      updateDialog: false,
      deleteDialog: false,
      headers: [
        {
          text: '#',
          value: 'index'
        },
        {
          text: 'ชื่อ-สกุล',
          value: 'TeacherName'
        },
        {
          text: 'Username',
          value: 'Username'
        },
        {
          text: '',
          value: 'actions'
        }
      ]
    }
  },
  async mounted() {
    await this.getTeachers()
  },
  methods: {
    async getTeachers() {
      let result = await this.$axios.$get('teacher.php', {
        params: {
          fn: 'All'
        }
      })
      if(result.Status==true) {
        this.teachers = JSON.parse(JSON.stringify(result.Data))
      }
    },
    async insertStatus(data) {
      if(data) {
        await this.getTeachers()
        this.insertDialog = false
      }
    },
    edit(item) {
      this.teacher = JSON.parse(JSON.stringify(item))
      //this.teacher = Object.assign({}, item)
      //this.teacher = structuredClone(item)
      this.updateDialog = true
    },
    async updateStatus(data) {
      if(data) {
        await this.getTeachers()
        this.updateDialog = false
      }
    },
    remove(item) {
      this.teacher = JSON.parse(JSON.stringify(item))
      this.deleteDialog = true
    },
    async deleteStatus(data) {
      if(data) {
        await this.getTeachers()
        this.deleteDialog = false
      }
    },
  }
}
</script>
