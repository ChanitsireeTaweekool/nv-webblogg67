<template>
  <div class="create-user-container">
    <h1>สร้างผู้ใช้ใหม่</h1>
    <form v-on:submit.prevent="createUser" class="user-form">
      <div class="form-group">
        <label for="name">ชื่อ:</label>
        <input type="text" id="name" v-model="user.name" class="form-control" placeholder="กรอกชื่อ">
      </div>
      <div class="form-group">
        <label for="lastname">นามสกุล:</label>
        <input type="text" id="lastname" v-model="user.lastname" class="form-control" placeholder="กรอกนามสกุล">
      </div>
      <div class="form-group">
        <label for="email">อีเมล:</label>
        <input type="email" id="email" v-model="user.email" class="form-control" placeholder="กรอกอีเมล">
      </div>
      <div class="form-group">
        <label for="password">รหัสผ่าน:</label>
        <input type="password" id="password" v-model="user.password" class="form-control" placeholder="กรอกรหัสผ่าน">
      </div>
      <div class="form-actions">
        <button type="submit" class="btn-submit">สร้างผู้ใช้</button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from '../../services/UsersService';
export default {
  data() {
    return {
      user: {
        name: '',
        lastname: '',
        email: '',
        password: '',
        status: 'active'
      }
    }
  },
  methods: {
    async createUser() {
      try {
        await UsersService.post(this.user);
        this.$router.push('/users');
      } catch (err) {
        console.log(err);
      }
    }
  }
}
</script>

<style scoped>
/* การตกแต่งคอนเทนเนอร์หลัก */
.create-user-container {
  max-width: 600px;
  margin: 30px auto;
  background: #f0f0f0;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

/* การตกแต่งฟอร์ม */
.user-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* การตกแต่งกล่องอินพุต */
.form-group {
  text-align: left;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

/* การจัดการปุ่ม */
.form-actions {
  text-align: center;
}

.btn-submit {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.btn-submit:hover {
  background-color: #45a049;
}
</style>
