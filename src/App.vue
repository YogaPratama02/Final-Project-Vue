<template>
  <v-app>
    <Alert />
    <Dialog />

    <div>
      <v-app-bar color="blue accent-4" dense dark>
        <v-toolbar-title
          ><h1 style="font-size: 24px">Sanbercode Blogs</h1></v-toolbar-title
        >

        <v-spacer></v-spacer>

        <v-btn
          v-for="(item, index) in menus"
          :key="`menu-${index}`"
          :to="item.route"
          class="menu-button"
          color="none"
          elevation="0"
          text
        >
          {{ item.title }}
        </v-btn>

        <v-menu left bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon v-bind="attrs" v-on="on">
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </template>

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
            <v-list-item v-if="guest">
              <v-btn block color="primary" class="mb-1" @click="login">
                <v-icon left>mdi-lock</v-icon>
                Login
              </v-btn>
            </v-list-item>

            <v-list-item v-if="guest">
              <v-btn block color="success" @click="register">
                <v-icon left>mdi-account</v-icon>
                Register
              </v-btn>
            </v-list-item>
            <v-list-item v-if="!guest">
              <v-btn block color="red" dark @click="logout">
                <v-icon left>mdi-lock</v-icon>
                logout
              </v-btn>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-app-bar>
    </div>

    <v-main style="width: 100%">
      <v-container fluid>
        <v-slide-y-transition>
          <router-view></router-view>
        </v-slide-y-transition>
      </v-container>
    </v-main>

    <v-footer app>
      <p>
        <a href="https://github.com/YogaPratama02">@YogaPratama02</a> |
        <a href="https://github.com/ariqsyahalam">@ariqsyahalam</a>
      </p>
    </v-footer>
  </v-app>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "App",
  components: {
    Alert: () => import("./components/Alert"),
    Dialog: () => import("./components/Dialog"),
  },
  data: () => ({
    drawer: true,
    menus: [
      { title: "Home", icon: "mdi-home", route: "/" },
      { title: "Blogs", icon: "mdi-note", route: "/blogs" },
    ],
    apiDomain: "http://demo-api-vue.sanbercloud.com",
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
      setAlert: "alert/set",
      setDialogComponent: "dialog/setComponent",
      setToken: "auth/setToken",
      setUser: "auth/setUser",
      checkToken: "auth/checkToken",
    }),
    register() {
      this.setDialogComponent({ component: "register" });
    },
  },
  mounted() {
    this.snackbarStatus = true;
    if (this.token) {
      this.checkToken(this.token);
    }
  },
};
</script>
<style lang="scss">
.v-navigation-drawer__content {
  background-color: rgb(255, 255, 255);
}

.menu-button {
  background-color: transparent !important;
  padding: 64px;

  color: white;
}
</style>
