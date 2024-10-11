<template>
  <div class="login-container">
    <br> </br>
    <h1>User Login</h1>
    <form v-on:submit.prevent="onLogin" class="login-form">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" v-model="email" required class="form-control" />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" v-model="password" required class="form-control" />
      </div>
      <button type="submit" class="btn-submit">Login</button>
      <div class="error" v-if="error">{{ error }}</div>
    </form>
  </div>
</template>

<script>
import AuthenService from "../services/AuthenService";
export default {
  data() {
    return {
      email: "",
      password: "",
      error: null,
    };
  },
  methods: {
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });

        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);

        this.$router.push({
          name: "users",
        });
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error;
        this.email = "";
        this.password = "";
      }
    },
  },
};
</script>

<style scoped>
.login-container {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  background: #2c2c2c; /* สีพื้นหลัง */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: #ffffff; /* สีตัวอักษร */
  text-align: center;
}

.login-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
  text-align: left;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #3a3a3a; /* สีพื้นหลังของ input */
  color: #ffffff; /* สีตัวอักษรใน input */
  transition: border-color 0.3s;
}

.form-control:focus {
  border-color: #4CAF50; /* สีเมื่อ input ถูกเลือก */
  outline: none; /* ไม่ให้แสดง outline */
}

.btn-submit {
  background-color: #4CAF50; /* สีสำหรับปุ่ม Login */
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-submit:hover {
  background-color: #45a049; /* สีเมื่อเมาส์ชี้ */
}

.error {
  color: red;
  margin-top: 10px;
  font-size: 0.9em; /* ขนาดตัวอักษร */
}
</style>
