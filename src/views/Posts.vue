<template>
  <div class="users">
    <div class="row justify-content-end">
      <div class="col-6">
        <h1>{{ message }}</h1>
      </div>
      <div class="col-3">
        <button class="btn btn-vuejs">Create New Post</button>
      </div>
    </div>
      <div v-for="post in posts" class="row justify-content-center">
        <div class="col-6">
          <div class="card h-100">
            <div class="card-body">
              <h3 class="card-title">{{ post.title }}</h3>
              <p class="card-subtitle text-muted">{{ post.author }}</p>
              <p class="card-text">{{ post.content }}</p>
            </div>
          </div>
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
var axios = require('axios');

export default {
  data: function() {
    return {
      message: "Posts",
      posts: []
    };
  },
  created: function() {
    axios({
      method: 'get',
      url: 'http://localhost:3000/api/posts'
    }).then(function(response) {
      this.posts = response.data;
    }.bind(this));
  },
  methods: {},
  computed: {}
};
</script>