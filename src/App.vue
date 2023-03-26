<template>
  <div>
    <router-view :username="username" @login="onLogin" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      loggedIn: false,
      username: null,
    };
  },
  methods: {
    onLogin(username) {
      this.loggedIn = true;
      this.username = username;
      this.$router.push('/pomodoro');
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
