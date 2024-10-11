<template>
  <div class="edit-user-container">
    <h1>แก้ไขผู้ใช้</h1>
    <form v-on:submit.prevent="editUser" class="edit-user-form">
      <div class="form-group">
        <label for="name">ชื่อ:</label>
        <input type="text" id="name" v-model="user.name" class="form-control" />
      </div>
      <div class="form-group">
        <label for="lastname">นามสกุล:</label>
        <input type="text" id="lastname" v-model="user.lastname" class="form-control" />
      </div>
      <div class="form-group">
        <label for="email">อีเมล:</label>
        <input type="email" id="email" v-model="user.email" class="form-control" />
      </div>
      <div class="form-group">
        <label for="password">รหัสผ่าน:</label>
        <input type="password" id="password" v-model="user.password" class="form-control" />
      </div>
      <div class="form-actions">
        <button type="submit" class="btn-submit">แก้ไขผู้ใช้</button>
        <button type="button" v-on:click="navigateTo('/users')" class="btn-cancel">กลับ</button>
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
        status: 'active',
      },
    };
  },
  async created() {
    try {
      var userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    async editUser() {
      try {
        await UsersService.put(this.user);
        this.$router.push('/users');
      } catch (err) {
        console.log(err);
      }
    },
    navigateTo(route) {
      this.$router.push(route);
    },
  },
};
</script>

<style scoped>
.edit-user-container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  background: #2c2c2c; /* สีพื้นหลัง */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: #ffffff; /* สีตัวอักษร */
  text-align: center;
}

.edit-user-form {
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
}

.form-actions {
  display: flex;
  justify-content: space-between;
}

.btn-submit {
  background-color: #4CAF50; /* สีสำหรับปุ่มแก้ไขผู้ใช้ */
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-submit:hover {
  background-color: #45a049; /* สีเมื่อเมาส์ชี้ */
}

.btn-cancel {
  background-color: #f44336; /* สีสำหรับปุ่มกลับ */
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancel:hover {
  background-color: #d32f2f; /* สีเมื่อเมาส์ชี้ */
}
</style>
