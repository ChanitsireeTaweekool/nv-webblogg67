<template>
  <div>
    <div class="nv-navbar">
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">Home</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>
        <!-- แสดงเมนู Login หรือ Logout ตามสถานะการเข้าสู่ระบบ -->
        <li v-if="isLogin()">
          <a href="#" @click.prevent="logout">Logout</a>
        </li>
        <li v-else>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    isLogin() {
      return this.$store.getters.isUserLoggedIn;
    },
    logout() {
      this.$store.dispatch("logout");
      this.$router.push({ name: "login" });
    }
  }
};
</script>

<style scoped>
/* การตกแต่งแถบเมนู */
.nv-navbar {
  background-color: #2c2c2c;
  width: 100%;
  padding: 15px 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  transition: background-color 0.3s;
}

/* การจัดการเมนู */
.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

/* การจัดการรายการเมนู */
.nv-navbar .nav li {
  margin: 0 15px;
}

/* การตกแต่งลิงก์ */
.nv-navbar .nav li a {
  padding: 10px 20px;
  text-decoration: none;
  color: #d3d3d3;
  font-weight: bold;
  font-size: 16px;
  position: relative;
  transition: color 0.3s, transform 0.3s;
}

/* การเปลี่ยนสีเมื่อชี้เมาส์ */
.nv-navbar .nav li a:hover {
  color: #ffffff;
  transform: scale(1.05);
}

/* การตกแต่งลิงก์ที่กำลังใช้งานอยู่ */
.nv-navbar .nav li a.router-link-active {
  color: #ffffff;
  border-bottom: 2px solid #ffffff;
}

/* เอฟเฟกต์แถบเมนูเมื่อเลื่อน */
body {
  padding-top: 60px;
}

body.scrolled .nv-navbar {
  background-color: #1f1f1f;
}
</style>
