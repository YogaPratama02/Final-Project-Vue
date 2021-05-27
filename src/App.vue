<template>
  <v-app>
    <Alert />
    <Dialog />
    <v-navigation-drawer app v-model="drawer">
      <v-list>
        <v-list-item v-if="!guest">
          <v-list-item-avatar>
            <v-img
              :src="
                user.photo_profile
                  ? apiDomain + user.photo_profile
                  : 'https://randomUser.me/api/portraits/men/78.jpg'
              "
            ></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title>{{ user.name }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <div class="pa-2" v-if="guest">
          <v-btn block color="primary" class="mb-1" @click="login">
            <v-icon left>mdi-lock</v-icon>
            Login
          </v-btn>
          <v-btn block color="success" @click="register">
            <v-icon left>mdi-account</v-icon>
            Register
          </v-btn>
        </div>

        <v-divider></v-divider>

        <v-list-item
          v-for="(item, index) in menus"
          :key="`menu-${index}`"
          :to="item.route"
        >
          <v-list-item-icon>
            <v-icon left>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append v-if="!guest">
        <div class="pa-2">
          <v-btn block color="red" dark @click="logout">
            <v-icon left>mdi-lock</v-icon>
            logout
          </v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <v-app-bar app color="success" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Sanbercode</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid>
        <!-- If using vue-router -->
        <v-slide-y-transition>
          <router-view></router-view>
        </v-slide-y-transition>
      </v-container>
    </v-main>

    <v-footer app>
      <p>
        @YogaPratama |
        <a href="https://github.com/ariqsyahalam">@ariqsyahalam</a>
      </p>
    </v-footer>
  </v-app>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
export default {
  // components: {Alert}, cara lain seperti dibawah
  name: "App",
  components: {
<<<<<<< HEAD
    Alert: () => import('./components/Alert'),
    Dialog: () => import('./components/Dialog'),
=======
    Alert: () => import("./components/Alert"),
    Dialog: () => import("./components/Dialog"),
>>>>>>> 51b551945468a9cbac7d7b1d37beab1539528620
  },
  data: () => ({
    drawer: true,
    menus: [
      { title: "Home", icon: "mdi-home", route: "/" },
      { title: "Blogs", icon: "mdi-note", route: "/blogs" },
    ],
    apiDomain: "http://demo-api-vue.sanbercloud.com",
    // snackbarStatus: false,
    // snackbarText: "Anda berhasil login",
    // guest: true
  }),
  computed: {
    ...mapGetters({
      guest: "auth/guest",
      user: "auth/user",
      token: "auth/token",
    }),
  },
  methods: {
    logout() {
      let config = {
        method: "post",
        url: "http://demo-api-vue.sanbercloud.com/api/v2/auth/logout",
        headers: {
          Authorization: "Bearer " + this.token,
        },
      };

      this.axios(config)
        .then(() => {
          this.setToken("");
          this.setUser({});
          this.setAlert({
            status: true,
            color: "success",
            text: "Anda berhasil logout",
          });
        })
        .catch((response) => {
          console.log(response);
        });
    },
    login() {
      this.setDialogComponent({ component: "login" });
    },
    ...mapActions({
<<<<<<< HEAD
      setAlert: 'alert/set',
      setDialogComponent: 'dialog/setComponent',
      setToken: 'auth/setToken',
      setUser: 'auth/setUser',
      checkToken: 'auth/checkToken',
    }),
    register(){
      this.setDialogComponent({'component' : 'register'})
    },
  },
  mounted(){
    // this.snackbarStatus = true
    if(this.token){
      this.checkToken(this.token)
=======
      setAlert: "alert/set",
      setDialogComponent: "dialog/setComponent",
      setToken: "auth/setToken",
      setUser: "auth/setUser",
      checkToken: "auth/checkToken",
    }),
  },
  mounted() {
    this.snackbarStatus = true;
    if (this.token) {
      this.checkToken(this.token);
>>>>>>> 51b551945468a9cbac7d7b1d37beab1539528620
    }
  },
};
</script>

<style>
.v-navigation-drawer__content {
<<<<<<< HEAD
  background-color: #fff1e6;
}
.v-toolbar__content{
  background-color: #577590;
=======
  background-color: rgb(255, 255, 255);
>>>>>>> 51b551945468a9cbac7d7b1d37beab1539528620
}
</style>
