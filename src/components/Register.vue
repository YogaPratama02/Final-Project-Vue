<template>
  <v-card class="card-wrapper">
    <v-toolbar dark color="success">
      <v-btn icon dark @click.native="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Register</v-toolbar-title>
    </v-toolbar>
    <v-divider></v-divider>
    <v-container fluid class="container-wrapper">
      <v-form ref="form">
        <v-text-field
          v-model="name"
          label="Name"
          required
          append-icon="mdi-account"
        ></v-text-field>
        <v-text-field
          v-model="email"
          label="E-mail"
          required
          append-icon="mdi-email"
        ></v-text-field>
        <v-text-field
          v-model="password"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          :type="showPassword ? 'text' : 'password'"
          required
          label="password"
          counter
          @click:append="showPassword = !showPassword"
        ></v-text-field>
        <v-file-input
          v-model="photo"
          counter
          show-size
          accept="image/*"
          truncate-length="14"
          label="Foto Profile"
        ></v-file-input>
        <div class="text-xs-center">
          <v-btn color="success lighten-1" @click="register">
            Register
            <v-icon right dark>mdi-lock-open</v-icon>
          </v-btn>
        </div>
      </v-form>
    </v-container>
  </v-card>
</template>

<script>
import { mapActions } from "vuex";
export default {
  data() {
    return {
      name: "",
      email: "",
      showPassword: false,
      password: "",
      photo: [],
      apiDomain: "https://demo-api-vue.sanbercloud.com",
    };
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setToken: "auth/setToken",
    }),
    close() {
      this.$emit("closed", false);
    },
    register() {
      // let file_input = this.$refs.photo.files[0]
      // console.log(file_input);
      let formData = new FormData();
      formData.append("name", this.name);
      formData.append("email", this.email);
      formData.append("password", this.password);
      formData.append("photo_profile", this.photo);
      // console.log(formData);

      const config = {
        method: "post",
        url: this.apiDomain + "/api/v2/auth/register",
        data: formData,
      };

      this.axios(config)
        .then((response) => {
          console.log(response.data);
          this.close();
        })
        .catch((response) => {
          console.log(response);
        });
    },
  },
};
</script>

<style lang="scss">
.card-wrapper {
  .container-wrapper {
    width: 75%;
    max-width: 500px;
  }
}
</style>