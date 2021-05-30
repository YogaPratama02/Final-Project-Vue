<template>
  <div>
    <div v-if="newBlog.add">
      <v-text-field
        type="textarea"
        v-model="newBlog.title"
        placeholder="title..."
        required
      ></v-text-field>
      <v-textarea
        type="textarea"
        auto-grow
        v-model="newBlog.description"
        placeholder="description..."
        required
      ></v-textarea>
      <v-btn color="grey" @click="clickNewBlog()">Cancel</v-btn>
      <v-btn color="green accent-4 ml-3" @click="createNewBlog()">
        <v-icon left>mdi-plus</v-icon>Create</v-btn
      >
    </div>
    <v-container v-if="!newBlog.add" class="grid-list-sm">
      <v-subheader> <h1 class="subheader-wrapper">Blogs</h1> </v-subheader>
      <v-btn
        v-if="!guest && !newBlog.add"
        color="success"
        class="mb-1 pa-2"
        @click="clickNewBlog()"
      >
        <v-icon left>mdi-plus</v-icon>
        Create New Blog
      </v-btn>
      <v-layout wrap class="blogitems-wrapper">
        <blog-item-component
          v-for="blog in blogs"
          :key="`blog-` + blog.id"
          :blog="blog"
        >
        </blog-item-component>
      </v-layout>
      <v-pagination
        v-model="page"
        @input="go"
        :length="lengthPage"
        :total-visible="perPage"
      ></v-pagination>
    </v-container>
  </div>
</template>

<script>
import BlogItemComponent from "../components/BlogItemComponent.vue";
import { mapActions, mapGetters } from "vuex";
const FormData = require("form-data");

export default {
  data: () => ({
    apiDomain: "http://demo-api-vue.sanbercloud.com",
    blogs: [],
    page: 0,
    lengthPage: 0,
    perPage: 0,
    newBlog: {
      add: false,
      title: "",
      description: "",
    },
  }),
  components: {
    "blog-item-component": BlogItemComponent,
  },
  computed: {
    ...mapGetters({
      guest: "auth/guest",
      user: "auth/user",
      token: "auth/token",
    }),
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setToken: "auth/setToken",
    }),
    go() {
      const config = {
        method: "get",
        url: this.apiDomain + "/api/v2/blog?page=" + this.page,
      };
      this.axios(config)
        .then((response) => {
          let { blogs } = response.data;
          this.blogs = blogs.data;
          this.page = blogs.current_page;
          this.lengthPage = blogs.last_page;
          this.perPage = blogs.per_page;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    clickNewBlog() {
      this.newBlog.add = !this.newBlog.add;
    },
    createNewBlog() {
      let form = new FormData();
      form.append("title", this.newBlog.title);
      form.append("description", this.newBlog.description);
      const config = {
        method: "post",
        url: `${this.apiDomain}/api/v2/blog`,
        data: form,
        headers: {
          Authorization: "Bearer " + this.token,
          Accept: "application/json",
        },
      };
      this.axios(config)
        .then(() => {
          this.clickNewBlog();
          this.setAlert({
            status: true,
            color: "success",
            text: "Berhasil menambahkan data",
          });
          this.newBlog.title = "";
          this.newBlog.description = "";
          this.go();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  created() {
    this.go();
  },
};
</script>

<style lang="scss">
.subheader-wrapper {
  font-size: 28px;
  font-weight: bold;
}
.blogitems-wrapper {
  padding: 16px;
}
</style>

