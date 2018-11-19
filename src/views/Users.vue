<template>
  <div class="users">
    <h1>{{ message }}</h1>
    <div class="row">
      <div v-for="user in users" class="col my-2">
        <div class="card h-100">
          <div class="card-body">
            <h3 class="card-title">{{ user.first_name }} {{ user.last_name }}</h3>
            <p class="card-subtitle text-muted">{{ user.email }}</p>
            <p class="card-text">{{ showCohortName(user) }}</p>
            <p class="card-text">Posts: {{ user.post_count }}</p>
          </div>
        </div>
      </div>
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
  methods: {
    showCohortName: function(user) {
      if (user.cohort) {
        return user.cohort.name;
      }
    }
  },
  computed: {}
};
</script>