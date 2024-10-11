<template>
    <div class="blog-container">
      <br />
      <h2>จัดการบล็อก</h2>
      <div class="actions">
        <button class="btn-logout" v-on:click="logout">ออกจากระบบ</button>
        <button class="btn-create" v-on:click="navigateTo('/blog/create')">สร้างบล็อกใหม่</button>
      </div>
      <h4>จำนวนบล็อก: {{ blogs.length }}</h4>
      <div v-for="blog in blogs" v-bind:key="blog.id" class="blog-item">
        <div class="blog-content">
          <p><strong>ID:</strong> {{ blog.id }}</p>
          <p><strong>ชื่ออาหาร:</strong> {{ blog.title }}</p>
          <p><strong>ส่วนผสม:</strong> {{ blog.category }}</p>
          <p><strong>ขั้นตอนการทำ:</strong> {{ blog.status }}</p>
          <p><strong>ผู้ให้สูตร:</strong> {{ blog.giver }}</p> <!-- ฟิลด์ใหม่ที่เพิ่มเข้ามา -->
        </div>
        <div class="blog-actions">
          <button class="btn-view" v-on:click="navigateTo('/blog/' + blog.id)">ดูบล็อก</button>
          <button class="btn-edit" v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไขบล็อก</button>
          <button class="btn-delete" v-on:click="deleteBlog(blog)">ลบข้อมูล</button>
        </div>
        <hr />
      </div>
    </div>
  </template>
  
  <script>
  import BlogsService from "@/services/BlogsService";
  
  export default {
    data() {
      return {
        blogs: [],
      };
    },
    async created() {
      try {
        const response = await BlogsService.index();
        this.blogs = response.data;
      } catch (err) {
        console.error(err);
      }
    },
    methods: {
      logout() {
        this.$store.dispatch("setToken", null);
        this.$store.dispatch("setBlog", null);
        this.$router.push({ name: "login" });
      },
      navigateTo(route) {
        this.$router.push(route);
      },
      async deleteBlog(blog) {
        const result = confirm("ต้องการลบข้อมูลนี้หรือไม่?");
        if (result) {
          try {
            await BlogsService.delete(blog);
            this.refreshData();
          } catch (err) {
            console.error(err);
          }
        }
      },
      async refreshData() {
        try {
          const response = await BlogsService.index();
          this.blogs = response.data;
        } catch (err) {
          console.error(err);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* การจัดการ container ของบล็อก */
  .blog-container {
    max-width: 800px;
    margin: auto;
    background: #2c2c2c; /* สีพื้นหลังของ container */
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    color: #ffffff; /* สีตัวอักษร */
    text-align: center;
  }
  
  /* การจัดการปุ่ม */
  .actions {
    margin-bottom: 20px;
    display: flex;
    justify-content: space-between;
  }
  
  /* ปุ่มออกจากระบบและสร้างบล็อก */
  .btn-logout,
  .btn-create {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
  }
  
  .btn-logout {
    background-color: #f44336; /* สีแดงสำหรับ logout */
  }
  
  .btn-create {
    background-color: #ffffff; /* สีน้ำเงินสำหรับสร้างบล็อกใหม่ */
  }
  
  /* ปุ่มดู แก้ไข และลบข้อมูล */
  .btn-view,
  .btn-edit,
  .btn-delete {
    padding: 8px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-right: 5px;
    font-size: 14px;
  }
  
  .btn-view {
    background-color: #000000; /* สีเขียวสำหรับดูบล็อก */
    color: white;
  }
  
  .btn-edit {
    background-color: #f0d86c; /* สีเหลืองสำหรับแก้ไขบล็อก */
    color: rgb(0, 0, 0);
  }
  
  .btn-delete {
    background-color: #f44336; /* สีแดงสำหรับลบข้อมูล */
    color: white;
  }
  
  /* การจัดการแต่ละบล็อก */
  .blog-item {
    background: #3a3a3a; /* สีพื้นหลังของแต่ละบล็อก */
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    text-align: left;
  }
  
  .blog-content p {
    margin: 5px 0;
  }
  
  /* การจัดการการกระทำของบล็อก */
  .blog-actions {
    margin-top: 10px;
    text-align: right;
  }
  </style>
  