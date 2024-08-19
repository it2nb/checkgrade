<template>
  <div>
    <v-form @submit.prevent="login">
    <v-card class="col-10 col-md-5 mx-auto mt-5">
      <v-card-title class="font-weight-bold">
        เข้าสู่ระบบ
      </v-card-title>
      <v-card-text>
        <v-row no-gutters class="mt-3">
          <v-col cols="11" class="mx-auto">
            <v-text-field
              label="ชื่อผู้ใช้"
              v-model="Username"
              outlined
            ></v-text-field>
          </v-col>
          <v-col cols="11" class="mx-auto">
            <v-text-field
              label="รหัสผ่าน"
              v-model="Password"
              type="password"
              outlined
            ></v-text-field>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions class="mb-2">
        <v-btn
          type="submit"
          color="success"
          class="mx-auto col-4"
          large
        >ลงชื่อเข้าใช้</v-btn>
      </v-card-actions>
    </v-card>
    </v-form>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
export default {
  data() {
    return {
      Username: "",
      Password: "",
    }
  },
  methods: {
    async login() {
      // let result = await this.$axios.$get('teacher.php', {
      //   params: {
      //     'fn': 'Login',
      //     'Username': this.Username,
      //     'Password': this.Password
      //   }
      // })
      let result = await this.$axios.$post('teacher.login.php', {
        'fn': 'Login',
        'Username': this.Username,
        'Password': this.Password
      })

      if(result.Status == true) {
        let userLogin = result.Data
        localStorage.setItem('userLogin', JSON.stringify(userLogin))
        if(userLogin.isAdmin == 1) {
          this.$router.replace('/Admin')
        } else {
          this.$router.replace('/Teacher')
        }
      } else {
        Swal.fire({
          icon: 'error',
          text: 'ชื่อผู้ใช้ หรือ รหัสผ่าน ไม่ถูกต้อง'
        })
      }
    }
  }
}
</script>
