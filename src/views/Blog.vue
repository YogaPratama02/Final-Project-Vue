<template>
  <div class="class-wrapper">
    <v-card v-if="blog.id" class="card-wrapper">
      <div v-if="!guest && !isEdit">
        <button @click="edit()">Edit</button>
        <button @click="del()">Delete</button>
      </div>
      <div v-if="!guest && isEdit">
        <button @click="update()">Update</button>
        <button @click="edit()">Cancel</button>
      </div>
      <h1 v-if="!isEdit">
        {{ blog.title.toUpperCase() }}
      </h1>
      <input v-else type="text" v-model="blog.title" />
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

      <p v-if="!isEdit">
        {{ blog.description }}
      </p>
      <input v-else type="textarea" v-model="blog.description" />
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
import { mapGetters } from "vuex";
const FormData = require("form-data");

export default {
  data: () => ({
    blog: {},
    apiDomain: "http://demo-api-vue.sanbercloud.com",
    isEdit: false,
  }),
  computed: {
    ...mapGetters({
      guest: "auth/guest",
      user: "auth/user",
      token: "auth/token",
    }),
  },
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
    edit() {
      this.isEdit = !this.isEdit;
    },
    update() {
      let { id } = this.$route.params;
      let form = new FormData();
      form.append("title", this.blog.title);
      form.append("description", this.blog.description);
      const config = {
        method: "post",
        url: `${this.apiDomain}/api/v2/blog/${id}?_method=PUT`,
        data: form,
        headers: {
          Authorization: "Bearer " + this.token,
          Accept: "application/json",
        },
      };
      this.axios(config)
        .then(() => {
          this.edit();
          this.go();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    del () {
      let { id } = this.$route.params;
      const config = {
        method: "post",
        url: `${this.apiDomain}/api/v2/blog/${id}?_method=DELETE`,
        headers: {
          Authorization: "Bearer " + this.token,
        },
      };
      this.axios(config)
        .then(() => {
          this.$router.replace("/")
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
