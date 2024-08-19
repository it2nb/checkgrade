<template>
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
</template>

<script>
export default {
  data() {
    return {
      teachers: [],
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
    }
  }
}
</script>
