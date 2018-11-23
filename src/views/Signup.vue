<template>
  <div class="signup">
    <div class="container">
      <div class="row">
        <div class="col-6 mx-auto">
          <form v-on:submit.prevent="submit()">
            <h1>Signup</h1>
            <ul>
              <li class="text-danger" v-for="error in errors">{{ error }}</li>
            </ul>
            <div class="form-group">
              <label>Cohort:</label>
              <select class="custom-select" v-model="cohortId" >
                <option v-for="cohort in cohorts" v-bind:value="cohort.id">{{cohort.name}}</option>
              </select>
            </div>
            <div class="form-group">
              <label>First Name:</label>
              <input type="text" class="form-control" v-model="firstName">
            </div>
            <div class="form-group">
              <label>Last Name:</label>
              <input type="text" class="form-control" v-model="lastName">
            </div>
            <div class="form-group">
              <label>Email:</label>
              <input type="email" class="form-control" v-model="email">
            </div>
            <div class="form-group">
              <label>Password:</label>
              <input type="password" class="form-control" v-model="password">
            </div>
            <div class="form-group">
              <label>Password confirmation:</label>
              <input type="password" class="form-control" v-model="passwordConfirmation">
            </div>
            <input type="submit" class="btn btn-primary" value="Submit">
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      firstName: "",
      lastName: "",
      email: "",
      cohortId: -1,
      password: "",
      passwordConfirmation: "",
      errors: [],
      cohorts: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/cohorts").then(response => {
      this.cohorts = response.data;
    }).catch(error => {
      this.errors = error.response.data.errors;
    });
  },
  methods: {
    submit: function() {
      if (this.cohortId !== -1) {
        var params = {
          first_name: this.firstName,
          last_name: this.lastName,
          cohort_id: this.cohortId,
          email: this.email,
          password: this.password,
          password_confirmation: this.passwordConfirmation
        };
        axios
          .post("http://localhost:3000/api/users", params)
          .then(response => {
            this.$router.push("/login");
          })
          .catch(error => {
            this.errors = error.response.data.errors;
          });
      } else {
        this.errors = ["Cohort Must Be Selected"];
      }
    }
  }
};
</script>