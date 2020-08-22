<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <br />
    <input type="file" name id @change="onFileSelected" />
    <button @click="upload">Upload</button>
    <span v-if="isInUpload">{{uploadPercentage}}%</span>
    <br />
    <div v-if="img">
      <p>Link file : {{img}}</p>
      <p>Uploaded Image :</p>
      <img :src="img" alt="uploaded image" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data: () => ({
    img: null,
    selectedFile: null,
    isInUpload: false,
    uploadPercentage: 0
  }),
  methods: {
    onFileSelected() {
      this.selectedFile = event.target.files[0];
    },
    upload() {
      const fD = new FormData();
      fD.append("image", this.selectedFile, this.selectedFile.name);
      axios
        .post(
          "https://api.imgbb.com/1/upload?key=3a9c096a26e849b4b39ddb680655c233",
          fD,
          {
            onUploadProgress: uploadEvent => {
              this.uploadPercentage = 0;
              this.isInUpload = true;
              this.uploadPercentage = Math.round(
                (uploadEvent.loaded / uploadEvent.total) * 100
              );
              // console.log(percentage);
            }
          }
        )
        .then(res => (this.img = res.data.data.display_url))
        .catch(err => console.log(err));
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
