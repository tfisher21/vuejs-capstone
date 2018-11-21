<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/users">Users</router-link> |
      <router-link to="/signup">Signup</router-link> |
      <router-link to="/login">Login</router-link> |
      <router-link to="/logout">Logout</router-link> |
    </div>
      <div class="container-fluid">
        <div class="row">
          <div class="col-3">
            <div class="row">
              <div v-if="user != {}" class="card w-100 h-100">
                <div class="card-body">
                  <h5 class="card-title">{{ user.first_name }} {{ user.last_name }}</h5>
                </div>
              </div>
            </div>
          </div>
          <div class="col-9">
            <router-view/>
          </div>
        </div>
      </div>
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
export default {
  template: "#login-page",
  data: function() {
    return {
      user: {}
    };
  },
  created: function() {
    console.log(localStorage.user);
    this.user = JSON.parse(localStorage.user);
  },
  methods: {}
};
</script>
