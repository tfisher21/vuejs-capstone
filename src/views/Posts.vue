<template>
  <div id="posts">
    <h1>{{ message }}</h1>

    <div class="row justify-content-center">
      <div class="col-6">
        <div class="card">
          <div class="card-header">
            <ul class="nav nav-pills nav-justified">
              <li class="nav-item">
                <a class="nav-link" href="#" v-on:click="showCohortPosts()"
                  >Cohort Posts</a
                >
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#" v-on:click="showAllPosts()"
                  >All Posts</a
                >
              </li>
            </ul>
          </div>
          <div class="card-body">
            <button
              class="btn btn-lg btn-vuejs w-100"
              data-toggle="collapse"
              href="#newPost"
              role="button"
              aria-expanded="false"
              aria-controls="newPost"
              v-on:click="showCreatePost = !showCreatePost"
            >
              <div v-if="showCreatePost">Close</div>
              <div v-else>Create New Post</div>
            </button>
            <div class="collapse mt-2" id="newPost">
              <form v-on:submit.prevent="publish()">
                <input
                  class="form-control form-control-lg my-1"
                  type="text"
                  v-model="newTitle"
                  placeholder="Title"
                />
                <textarea
                  class="form-control my-1"
                  cols="30"
                  rows="10"
                  v-model="newContent"
                  placeholder="Start writing something..."
                ></textarea>
                <div class="row justify-content-end">
                  <ul class="list-unstyled mt-2">
                    <li class="text-danger" v-for="error in errors">
                      {{ error }}
                    </li>
                  </ul>
                  <div class="col-4 text-right">
                    <input
                      type="button"
                      class="text-right btn btn-secondary mr-1"
                      value="Clear"
                      v-on:click="clearPost()"
                    />
                    <input
                      type="submit"
                      class="text-right btn btn-vuejs"
                      value="Publish!"
                    />
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-for="post in posts" class="row justify-content-center">
      <div class="col-6">
        <div class="card">
          <div class="card-body">
            <h3 class="card-title">{{ post.title }}</h3>
            <p class="card-subtitle text-muted">{{ post.author }}</p>
            <p class="card-text">{{ post.content }}</p>
            <button
              class="btn btn-vuejs"
              data-toggle="modal"
              v-on:click="currentPost = post"
              data-target="#comments"
            >
              Comments
            </button>
          </div>
        </div>
      </div>
    </div>
    <div
      class="modal fade"
      id="comments"
      tabindex="-1"
      role="dialog"
      aria-labelledby="commentsModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="commentsModalLabel">
              {{ currentPost.title }}
              <span class="text-muted">{{ currentPost.author }}</span>
            </h5>

            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div
              v-for="comment in currentPost.comments"
              class="row border-bottom"
            >
              <div class="col my-2">
                <div class="text-left text-vuejs">{{ comment.author }}</div>
                <div class="text-left pl-4">{{ comment.content }}</div>
                <div class="text-right text-muted">
                  <small>{{ comment.created_at }}</small>
                </div>
              </div>
            </div>
            <div class="row">
              <textarea
                class="form-control m-2"
                cols="30"
                rows="10"
                v-model="newComment"
                placeholder="Start writing something..."
              ></textarea>
            </div>
          </div>
          <div class="modal-footer">
            <form v-on:submit.prevent="publishComment(currentPost.id)">
              <button
                type="button"
                class="btn btn-sm btn-secondary mr-1"
                data-dismiss="modal"
              >
                Close
              </button>
              <button class="btn btn-sm btn-vuejs">Add a Comment</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
#posts {
  text-align: center;
}

.btn-vuejs {
  background-color: #42b983;
  color: white;
}

.text-vuejs {
  color: #42b983;
}
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Posts",
      posts: [],
      newTitle: "",
      newContent: "",
      errors: [],
      showCreatePost: false,
      currentPost: {},
      newComment: ""
    };
  },
  created: function() {
    var params = {
      sort_by_cohort: true
    };
    axios
      .get("https://capstone.tyler.fish/api/posts", { params })
      .then(response => {
        this.posts = response.data;
      });
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
      axios
        .post("https://capstone.tyler.fish/api/posts", params)
        .then(response => {
          // this.posts = [response.data].concat(this.posts);
          // this.posts.push(response.data);
          this.posts.unshift(response.data);
          this.newTitle = "";
          this.newContent = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    showAllPosts: function() {
      axios.get("https://capstone.tyler.fish/api/posts").then(response => {
        this.posts = response.data;
      });
    },
    showCohortPosts: function() {
      axios
        .get("https://capstone.tyler.fish/api/posts", {
          params: {
            sort_by_cohort: true
          }
        })
        .then(response => {
          this.posts = response.data;
        });
    },
    publishComment: function() {
      var params = {
        content: this.newComment,
        post_id: this.currentPost.id
      };
      console.log(this.currentPost.id);
      axios
        .post("https://capstone.tyler.fish/api/post_comments", params)
        .then(response => {
          console.log(response.data);
          this.currentPost.comments.push(response.data);
          this.newComment = "";
        })
        .catch(errors => {
          console.log(errors.response.data.errors);
          this.errors = errors.response.data.errors;
        });
    }
  },
  computed: {}
};
</script>
