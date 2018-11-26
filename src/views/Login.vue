<template>
  <div class="login">
    <div class="container">
      <div class="row">
        <div class="col-6 mx-auto">
          <form v-on:submit.prevent="submit()">
            <h1>Login</h1>
            <ul>
              <li class="text-danger" v-for="error in errors">{{ error }}</li>
            </ul>
            <div class="form-group">
              <label>Email:</label>
              <input type="email" class="form-control" v-model="email">
            </div>
            <div class="form-group">
              <label>Password:</label>
              <input type="password" class="form-control" v-model="password">
            </div>
            <input type="submit" class="btn btn-vuejs" value="Submit">
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.btn-vuejs {
  background-color: #42b983;
  color: white;
}
</style>
<script>
import axios from "axios";

export default {
  template: "#login-page",
  data: function() {
    return {
      email: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password
      };
      axios
        .post("http://localhost:3000/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$parent.jwt = true;
          this.$router.push("/posts");
        })
        .catch(error => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    }
  }
};
</script>