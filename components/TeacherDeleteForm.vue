<template>
  <v-form
    @submit.prevent="remove"
  >
    <v-card>
      <v-card-title class="text--error">
        <v-icon color="error" class="mr-1">fas fa-edit</v-icon>ลบข้อมูลครูผู้สอน
      </v-card-title>
      <v-card-text class="pa-5">
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field
              v-model="teacher.TeacherName"
              label="ชื่อ-สกุล"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="6">
            <v-text-field
              v-model="teacher.Username"
              label="ชื่อผู้ใช้"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12"><v-divider></v-divider></v-col>
          <v-col cols="12" class="text-center">
            <v-btn type="submit" color="error" class="px-5">ลบ</v-btn>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-form>
</template>

<script>
export default {
  props: {
    teacherdata: {
      type: Object,
      default: ()=>{}
    }
  },
  data(){
    return {
      teacher: {}
    }
  },
  mounted() {
    this.teacher = JSON.parse(JSON.stringify(this.teacherdata))
    delete this.teacher.Password
  },
  methods: {
    async remove() {
      this.teacher.fn = 'Delete'
      let result = await this.$axios.$post('teacher.delete.php', this.teacher)
      console.log(result)
      if(result.Status==true) {
        this.$emit('deleteStatus', true)
      }
    }
  }
}
</script>
