<template>
  <div class="users">
    <h1>{{ message }}</h1>
    <div class="row justify-content-center">
      <div class="col-6">

        <div class="card card-body">
          <button class="btn btn-lg btn-vuejs" data-toggle="collapse" href="#newPost" role="button" aria-expanded="false" aria-controls="newPost">Create New Post</button>
          <div class="collapse mt-2" id="newPost">
            <form v-on:submit.prevent="publish()">
              <input class="form-control form-control-lg my-1" type="text" v-model="newTitle" placeholder="Title">
              <textarea class="form-control my-1" cols="30" rows="10" v-model="newContent" placeholder="Start writing something..."></textarea>
              <div class="row justify-content-end">
                <ul class="list-unstyled mt-2">
                  <li class="text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
                <div class="col-4 text-right">
                  <input type="button" class="text-right btn btn-secondary mr-1" value="Clear" v-on:click="clearPost()">
                  <input type="submit" class="text-right btn btn-vuejs" value="Publish!">
                </div>
              </div>
            </form>
          </div>
        </div>
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
      posts: [],
      newTitle: "",
      newContent: "",
      errors: []
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
  methods: {
    clearPost: function() {
      this.newTitle = "";
      this.newContent = "";
    },
    publish: function() {
      console.log("publish button pressed");
      var params = {
        title: this.newTitle,
        content: this.newContent
      };
      axios.post("http://localhost:3000/api/posts", params).then(response => {
        this.posts = [response.data].concat(this.posts);
        // this.posts.push(response.data);
        this.newTitle = "";
        this.newContent = "";
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  },
  computed: {}
};
</script>