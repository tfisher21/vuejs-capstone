<template>
  <div class="home">
    <div class="container">
      <div class="jumbotron">
        <h1>{{ message }}</h1>
        <p class="lead">We want to help you communicate with your cohort and the graduates of the Actualize Bootcamp.</p>
        <hr class="my-4">
        <div id="homeLogin">
          <p>Please Login or Signup to get started!</p>
          <router-link class="btn btn-vuejs mx-1" to="/login">Login</router-link>
          <router-link class="btn btn-vuejs mx-1" to="/signup">Signup</router-link>
        </div>
      </div>
    </div>

  </div>
</template>

<style>
#homeLogin {
  text-align: center;
}

.btn-vuejs {
  background-color: #42b983;
  color: white;
}
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Actualize.social!"
    };
  },
  created: function() {},
  methods: {},
  computed: {}
};
</script>