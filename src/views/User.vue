<template>
  <div class="user">
    <div class="container">
      <div class="row">
        <div class="col-2">
          <img
            :src="user.avatar"
            class="img-fluid float-left border rounded-circle"
          />
        </div>
        <div class="col">
          <div class="h3">
            {{ user.first_name }}
            <span class="text-muted">{{ user.last_name }}</span>
          </div>
          <div>
            Member of <em>{{ user.cohort.name }}</em>
          </div>
          <div>
            {{ user.email }}
          </div>
          <div>
            {{ user.post_cohort_employer }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  props: ["id"],
  data: function() {
    return {
      message: "User",
      user: []
    };
  },
  created: function() {
    axios({
      method: "get",
      url: "http://capstone.tyler.fish/api/users/" + this.id
    }).then(
      function(response) {
        this.user = response.data;
      }.bind(this)
    );
  },
  methods: {},
  computed: {}
};
</script>
