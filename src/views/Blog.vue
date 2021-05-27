<template>
  <div class="class-wrapper">
    <v-card v-if="blog.id" class="card-wrapper">
      <h1>
        {{ blog.title.toUpperCase() }}
      </h1>
      <v-img
        :src="
          blog.photo ? apiDomain + blog.photo : 'https://picsum.photos/900/300/'
        "
        class="blog-image"
      >
      </v-img>
      <p class="sumber-foto">
        sumber :
        <a>{{ blog.photo ? apiDomain : "https://picsum.photos/" }}</a>
      </p>

      <p>
        {{ blog.description }}
      </p>
      <p class="lastupdate">
        Terakhir diperbarui pada tanggal {{ blog.updated_at.split(" ")[0] }}
      </p>
    </v-card>
    <v-card v-else-if="!blog.id" class="card-wrapper">
      <h1>Data belum ditemukan</h1>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    blog: {},
    apiDomain: "http://demo-api-vue.sanbercloud.com",
  }),
  methods: {
    go() {
      let { id } = this.$route.params;
      const config = {
        method: "get",
        url: `${this.apiDomain}/api/v2/blog/${id}`,
      };
      this.axios(config)
        .then((response) => {
          let { blog } = response.data;
          this.blog = blog;
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
.class-wrapper {
  padding: 64px;

  .card-wrapper {
    padding: 32px;
    .button-wrapper {
      padding-bottom: 16px;
    }
    .blog-image {
      display: block;
      margin-left: auto;
      margin-right: auto;
      max-height: 50%;
    }
    .sumber-foto {
      text-align: center;
      color: gray;
      font-size: 12px;
    }
    h1 {
      padding: 16px 0px;
    }

    p {
      text-align: justify;
    }

    .lastupdate {
      color: gray;
      font-size: 12px;
    }
  }
}
</style>
