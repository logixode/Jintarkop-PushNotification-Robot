<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <form @submit.prevent>
      <input type="number" name="id" v-model="id" placeholder="id" />
      <input type="text" name="nama" v-model="nama" placeholder="nama" />
      <input type="text" name="alamat" v-model="alamat" placeholder="alamat" />
      <br />
      <button type="submit" @click="add">add</button>
      <button type="submit" @click="update">update</button>
    </form>
    <ol>
      <li class v-for="data in datas" :key="data.id">
        <p>{{data.nama}} - {{data.alamat}}</p>
      </li>
    </ol>
  </div>
</template>

<script>
import axios from "axios";
import qs from "qs";

export default {
  name: "App",
  data: () => ({
    datas: {},
    headers: {
      headers: {
        "content-type": "application/x-www-form-urlencoded;charset=utf-8"
      }
    },
    id: "",
    nama: "",
    alamat: ""
  }),
  mounted() {
    axios
      .get("https://website-coba-api.000webhostapp.com/api/siswa")
      .then(res => (this.datas = res.data))
      .catch(err => console.log(err));
  },
  methods: {
    add() {
      axios
        .post(
          `https://website-coba-api.000webhostapp.com/api/siswa`,
          qs.stringify({
            nama: this.nama,
            alamat: this.alamat
          }),
          this.headers
        )
        .then(res => {
          this.datas.push(res.data);
        });
    },
    update() {
      axios.put(
        `https://website-coba-api.000webhostapp.com/api/siswa/${this.id}`,
        qs.stringify({
          nama: this.nama,
          alamat: this.alamat
        }),
        this.headers
      );
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
