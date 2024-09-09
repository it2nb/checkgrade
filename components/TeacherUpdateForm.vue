<template>
  <v-form
    @submit.prevent="update"
  >
    <v-card>
      <v-card-title class="text--warning">
        <v-icon color="warning" class="mr-1">fas fa-edit</v-icon>แก้ไขข้อมูลครูผู้สอน
      </v-card-title>
      <v-card-text class="pa-5">
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field
              v-model="teacher.TeacherName"
              label="ชื่อ-สกุล"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="6">
            <v-text-field
              v-model="teacher.Username"
              label="ชื่อผู้ใช้"
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-switch
              v-model="teacher.isAdmin"
              label="สิทธิ์ผู้ดูแลระบบ"
              :true-value="1"
              :false-value="0"
            ></v-switch>
          </v-col>
          <v-col cols="12"><v-divider></v-divider></v-col>
          <v-col cols="12" class="text-center">
            <v-btn type="submit" color="warning" class="px-5">แก้ไข</v-btn>
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
    async update() {
      this.teacher.fn = 'Update'
      let result = await this.$axios.$post('teacher.update.php', this.teacher)
      console.log(result)
      if(result.Status==true) {
        this.$emit('updateStatus', true)
      }
    }
  }
}
</script>
