<template>
  <v-flex class="flex-wrapper">
    <v-card :to="'/blog/' + blog.id" class="card-wrapper">
      <v-img
        :src="
          blog.photo ? apiDomain + blog.photo : 'https://picsum.photos/200/300'
        "
        class="white--text"
        height="200px"
      >
      </v-img>
      <v-card-actions>
        <h1 class="card-title">{{ blog.title }}</h1>
      </v-card-actions>
      <v-card-actions>
        <p class="card-desc">{{ blog.description }}</p>
      </v-card-actions>
    </v-card>
  </v-flex>
</template>

<script>
export default {
  data: () => ({
    apiDomain: "http://demo-api-vue.sanbercloud.com",
  }),
  props: ["blog"],
};
</script>

<style lang="scss">
.flex-wrapper {
  width: 250px;
  margin: 16px 8px;
  max-width: 300px;
}
.card-wrapper {
  padding: 32px 8px;
  height: 100%;
  .card-title {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    color: green;
    font-size: 18px;
  }
  .card-desc {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 3; /* number of lines to show */
    -webkit-box-orient: vertical;
    text-align: justify;
  }
}
</style>
