<template>
  <div class="class-wrapper">
    <v-card v-if="blog.id" class="card-wrapper">
      <div v-if="!guest && !isEdit">
        <v-btn color="green accent-4" @click="edit()"> <v-icon left>mdi-briefcase-upload</v-icon>Edit</v-btn>
        <v-btn color="red lighten-1 ml-3" @click="del()"><v-icon left>mdi-delete</v-icon>Delete</v-btn>
      </div>
      <div v-if="!guest && isEdit">
        <v-btn color="green accent-4" @click="update()"> <v-icon left>mdi-briefcase-upload</v-icon>Update</v-btn>
        <v-btn color="grey ml-3" @click="edit()">Cancel</v-btn>
      </div>
      <h1 v-if="!isEdit">
        {{ blog.title.toUpperCase() }}
      </h1>
      <v-text-field v-else type="text" v-model="blog.title"></v-text-field>
      <v-img
        :src="
          blog.photo ? apiDomain + blog.photo : 'https://picsum.photos/900/300/'
        "
        class="blog-image"
      >
      </v-img>
      <v-btn color="green accent-4 mt-2" v-if="!newPhoto" @click="clickNewPhoto()">
        Upload New Photo
      </v-btn>
      <div v-else class="mt-2">
        <v-file-input
          v-model="photo_profile"
          counter
          show-size
          accept="image/*"
          truncate-length="14"
          label="Update Foto Profile"
        ></v-file-input>
        <v-btn color="green accent-4" @click="uploadPhoto()"><v-icon left>mdi-briefcase-upload</v-icon>Upload</v-btn>
        <v-btn color="grey ml-3" @click="clickNewPhoto()">Cancel</v-btn>
      </div>
      <p class="sumber-foto">
        sumber :
        <a>{{ blog.photo ? apiDomain : "https://picsum.photos/" }}</a>
      </p>

      <p v-if="!isEdit">
        {{ blog.description }}
      </p>
      <v-textarea v-else auto-grow type="text" v-model="blog.description"></v-textarea>
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
import { mapActions ,mapGetters } from "vuex";
const FormData = require("form-data");

export default {
  data: () => ({
    blog: {},
    apiDomain: "https://demo-api-vue.sanbercloud.com",
    isEdit: false,
    newPhoto: false,
    photo: null,
    photo_profile: []
  }),
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
          this.setAlert({
            status: true,
            color: "success",
            text: "Berhasil mengubah data",
          });
          this.go();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    del() {
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
          this.$router.replace("/");
          this.setAlert({
            status: true,
            color: "red",
            text: "Berhasil menghapus data",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    clickNewPhoto() {
      this.newPhoto = !this.newPhoto;
    },
    uploadPhoto() {
      let { id } = this.$route.params;
      let form = new FormData();
      form.append("photo", this.photo_profile);
      const config = {
        method: "post",
        url: `${this.apiDomain}/api/v2/blog/${id}/upload-photo`,
        data: form,
        headers: {
          Authorization: "Bearer " + this.token,
        },
      };
      this.axios(config)
        .then(() => {
          this.clickNewPhoto();
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
