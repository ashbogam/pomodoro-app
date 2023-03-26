<template>
  <div class="login-container">
    <h2>Focus Up</h2>
    <form class="login-form" @submit.prevent="login">
      <div class="form-group">
        <label for="username">Username:</label>
        <input type="text" id="username" v-model="username" required>
      </div>
      <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <button type="submit" class="btn-login">Login</button>
    </form>
    <p v-if="error" class="error-message">{{ error }}</p>
  </div>
</template>


<script>
export default {
  data() {
    return {
      username: "",
      password: "",
      error: "",
    };
  },
  methods: {
    login() {

      if (this.username === "abogam" && this.password === "abogam@123") {
        localStorage.setItem("user", JSON.stringify({ username: this.username }));
        this.$router.push({ name: "PomodoroTimer" });
      } else {
        this.error = "Invalid username or password";
      }
    },
  },
};
</script>

<style>

.login-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f7f7f7;
}

.login-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  font-weight: bold;
}

input[type="text"],
input[type="password"] {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
}

.btn-login {
  background-color: #4CAF50;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.btn-login:hover {
  background-color: #3e8e41;
}

.error-message {
  color: red;
  font-size: 14px;
  margin-top: 10px;
}

</style>
