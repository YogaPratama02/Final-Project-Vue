<template>
  <v-container class="grid-list-sm">
    <v-subheader> <h1 class="subheader-wrapper">Blogs</h1> </v-subheader>
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
</template>

<script>
import BlogItemComponent from "../components/BlogItemComponent.vue";
export default {
  data: () => ({
    apiDomain: "http://demo-api-vue.sanbercloud.com",
    blogs: [],
    page: 0,
    lengthPage: 0,
    perPage: 0,
  }),
  components: {
    "blog-item-component": BlogItemComponent,
  },
  methods: {
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

