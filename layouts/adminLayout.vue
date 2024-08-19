<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>{{ title }}</v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'ข้อมูลส่วนตัว',
          to: '/Admin'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'ข้อมูลรายวิชา',
          to: '/Admin/subject'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'ข้อมูลผู้สอน',
          to: '/Admin/teacher'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'ข้อมูลผู้เรียน',
          to: '/Admin/student'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'ข้อมูลทะเบียนเรียน',
          to: '/Admin/enrollment'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'ระบบตรวจสอบผลการเรียน (ผู้ดูแลระบบ)'
    }
  }
}
</script>
