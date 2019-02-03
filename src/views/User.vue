<template>
  <div class="user">
    <h1>{{ $route.params.id }}</h1>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "User",
      user: []
    };
  },
  created: function() {
    // axios({
    //   method: "get",
    //   url: "http://capstone.tyler.fish/api/users/:id"
    // }).then(
    //   function(response) {
    //     this.user = response.data;
    //   }.bind(this)
    // );
  },
  methods: {},
  computed: {}
};
</script>
