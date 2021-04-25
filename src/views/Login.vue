<template>
  <div>
    <div>Usuario autenticado: {{ user.name }}</div>
    <button v-if="user.email" @click="logout">Logout</button>
    <form v-else @submit.prevent="login">
      <input v-model="form.email" type="email" placeholder="Email..." /><br />
      <input
        v-model="form.password"
        type="password"
        placeholder="Password..."
      /><br />
      <button>Login</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";
axios.defaults.withCredentials = true;
axios.defaults.baseURL = "http://localhost:251/";
export default {
  data: () => ({
    user: {},
    form: {
      email: "yicksonr@gmail.com",
      password: "password",
    },
  }),
  created() {
    axios
      .get("/api/user")
      .then((res) => {
        this.user = res.data;
      })
      .catch(() => {});
  },
  methods: {
    logout() {
      axios.post("/logout").then(() => {
        this.user = {};
      });
    },
    login() {
      axios.get("/sanctum/csrf-cookie").then(() => {
        axios.post("/login", this.form).then((res) => {
          this.user = res.data;
        });
      });
    },
  },
};
</script>
