<template>
  <div class="user-container">
    <h1>สมาชิกทั้งหมด</h1>
    <div class="action-buttons">
      <button class="btn-create" v-on:click="navigateTo('/user/create')">สร้างผู้ใช้</button>
    </div>
    <hr>
    <div v-if="users.length">
      <div class="user-count"><b>จำนวนผู้ใช้งาน:</b> {{ users.length }}</div>
      <div v-for="user in users" v-bind:key="user.id" class="user-item">
        <div class="user-info">
          <div><b>ID:</b> {{ user.id }}</div>
          <div><b>ชื่อผู้ใช้:</b> {{ user.name }} {{ user.lastname }}</div>
          <div><b>อีเมล:</b> {{ user.email }}</div>
          <div><b>สถานะ:</b> {{ user.status }}</div>
        </div>
        <div class="user-actions">
          <button class="btn-view" v-on:click="navigateTo('/user/' + user.id)">ดูข้อมูล</button>
          <button class="btn-edit" v-on:click="navigateTo('/user/edit/' + user.id)">แก้ไขข้อมูล</button>
          <button class="btn-delete" v-on:click="deleteUser(user)">ลบข้อมูล</button>
        </div>
        <hr>
      </div>
    </div>
    <div class="logout-button">
      <button class="btn-logout" v-on:click="logout">ออกจากระบบ</button>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";
export default {
  data() {
    return {
      users: []
    };
  },
  async created() {
    try {
      this.users = (await UsersService.index()).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    logout() {
      this.$store.dispatch("setToken", null);
      this.$store.dispatch("setUser", null);
      this.$router.push({
        name: "login"
      });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style scoped>
/* การตกแต่งคอนเทนเนอร์ */
.user-container {
  max-width: 900px;
  margin: 30px auto;
  background: #f0f0f0;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

/* การจัดการปุ่ม */
.action-buttons, .logout-button {
  margin-bottom: 20px;
}

.btn-create, .btn-logout, .btn-view, .btn-edit, .btn-delete {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  color: white;
}

/* สีปุ่มต่างๆ */
.btn-create {
  background-color: #4CAF50;
  margin-bottom: 15px;
}

.btn-logout {
  background-color: #f44336;
}

.btn-view {
  background-color: #2196F3;
}

.btn-edit {
  background-color: #FFC107;
}

.btn-delete {
  background-color: #f44336;
}

/* การตกแต่งข้อมูลผู้ใช้ */
.user-item {
  background: #fff;
  margin-bottom: 20px;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  text-align: left;
}

.user-info {
  margin-bottom: 10px;
}

.user-info div {
  margin: 5px 0;
}

/* การจัดการปุ่มในแต่ละข้อมูลผู้ใช้ */
.user-actions {
  display: flex;
  gap: 10px;
}

.btn-create:hover, .btn-logout:hover, .btn-view:hover, .btn-edit:hover, .btn-delete:hover {
  opacity: 0.8;
}
</style>
