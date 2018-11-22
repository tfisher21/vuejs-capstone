<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <router-link class="btn btn-vuejs mx-1" to="/signup">Signup</router-link>
    <router-link class="btn btn-vuejs mx-1" to="/login">Login</router-link>

  </div>
</template>

<style>
.btn-vuejs {
  background-color: #42b983;
  color: white;
}
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!"
    };
  },
  created: function() {},
  methods: {},
  computed: {}
};
</script>