<template>
  <v-form
    @submit.prevent="insert"
  >
    <v-card>
      <v-card-title class="text--success">
        <v-icon color="success" class="mr-1">fas fa-plus-circle</v-icon>เพิ่มข้อมูลครูผู้สอน
      </v-card-title>
      <v-card-text class="pa-5">
        <v-row>
          <v-col cols="12">
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
          <v-col cols="12" md="6">
            <v-text-field
              v-model="teacher.Password"
              type="password"
              label="รหัสผ่าน"
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
            <v-btn type="submit" color="success" class="px-5">เพิ่ม</v-btn>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-form>
</template>

<script>
export default {
  data(){
    return {
      teacher: {
        isAdmin: 0
      }
    }
  },
  methods: {
    async insert() {
      this.teacher.fn = 'Insert'
      let result = await this.$axios.$post('teacher.insert.php', this.teacher)
      console.log(result)
      if(result.Status==true) {
        this.$emit('insertStatus', true)
        this.teacher = {isAdmin: 0}
      }
    }
  }
}
</script>
