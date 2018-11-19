<template>
  <div class="users">
    <h1>{{ message }}</h1>
    <div v-for="user in users">
      <h3>{{ user.first_name }} {{ user.last_name }}</h3>
      <p>{{ user.email }}</p>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      message: "Users",
      users: []
    };
  },
  created: function() {
    axios({
      method: 'get',
      url: 'http://localhost:3000/api/users',
      headers: {
        'Authorization': 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjozLCJleHAiOjE1NDI2NjUzODJ9.WkTIpp7F_7kvjN8OLYbIqUA4ijLAHC5U1BF1AWtS9OE'
      }
    }).then(function(response) {
      this.users = response.data;
    }.bind(this));
  },
  methods: {},
  computed: {}
};
</script>