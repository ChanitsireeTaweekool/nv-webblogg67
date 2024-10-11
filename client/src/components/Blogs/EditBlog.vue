<template>
  <div class="edit-blog-container">
    <h1 class="header-title">แก้ไขสูตรอาหาร</h1>
    <form v-on:submit.prevent="editBlog" class="edit-blog-form">
      <div class="form-group">
        <label for="title">ชื่อเรื่อง:</label>
        <input
          type="text"
          id="title"
          v-model="blog.title"
          placeholder="กรอกชื่อบล็อก"
          class="form-control"
        />
      </div>

      <transition name="fade">
        <div class="thumbnail-preview" v-if="blog.thumbnail !== 'null'">
          <img :src="BASE_URL + blog.thumbnail" alt="รูปภาพตัวอย่าง" class="thumbnail-img" />
        </div>
      </transition>

      <div class="form-group">
        <label>อัปโหลดรูปภาพ:</label>
        <div class="dropbox">
          <input
            type="file"
            multiple
            :name="uploadFieldName"
            :disabled="isSaving"
            @change="filesChange($event.target.name, $event.target.files)"
            accept="image/*"
            class="input-file"
          />
          <p v-if="isInitial">เลือกไฟล์รูปภาพที่ต้องการอัปโหลด</p>
          <p v-if="isSaving">กำลังอัปโหลด {{ fileCount }} ไฟล์...</p>
          <p v-if="isSuccess">อัปโหลดสำเร็จแล้ว</p>
        </div>
      </div>

      <div class="form-group">
        <label for="category">ส่วนผสม:</label>
        <textarea
          id="category"
          v-model="blog.category"
          placeholder="กรุณากรอกส่วนผสม"
          class="form-textarea"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="status">ขั้นตอนการทำ:</label>
        <textarea
          id="status"
          v-model="blog.status"
          placeholder="กรุณากรอกขั้นตอนการทำ"
          class="form-textarea"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="author">ผู้ให้สูตร:</label>
        <input
          type="text"
          id="author"
          v-model="blog.author"
          placeholder="กรุณากรอกชื่อผู้ให้สูตร"
          class="form-control"
        />
      </div>

      <div class="form-group form-actions">
        <button type="submit" class="btn-submit">อัปเดตบล็อก</button>
        <button type="button" v-on:click="navigateTo('/blogs')" class="btn-cancel">กลับ</button>
      </div>
    </form>
  </div>
</template>

<script>
import BlogsService from "@/services/BlogsService";
import UploadService from "@/services/UploadService";

export default {
  data() {
    return {
      BASE_URL: "http://localhost:8081/assets/uploads/",
      uploadFieldName: "userPhoto",
      fileCount: 0,
      blog: {
        title: "",
        thumbnail: "null",
        category: "",
        status: "",
        author: "", // เพิ่มฟิลด์สำหรับผู้ให้สูตร
      },
    };
  },
  methods: {
    async editBlog() {
      try {
        await BlogsService.put(this.blog);
        this.$router.push({ name: "blogs" });
      } catch (err) {
        console.error("Failed to update blog:", err);
      }
    },
    filesChange(fieldName, fileList) {
      if (!fileList.length) return;
      this.fileCount = fileList.length;
      console.log("Files selected:", fileList);
    },
    navigateTo(route) {
      this.$router.push(route);
    },
  },
};
</script>

<style scoped>
.edit-blog-container {
  max-width: 800px;
  margin: auto;
  background: #2c2c2c; /* สีพื้นหลัง */
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: #ffffff; /* สีตัวอักษร */
}

.header-title {
  text-align: center; /* จัดกลางข้อความ */
  margin-bottom: 20px; /* เพิ่มระยะห่างใต้หัวข้อ */
}

.edit-blog-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #3a3a3a; /* สีพื้นหลังของ input */
  color: #ffffff; /* สีตัวอักษรใน input */
}

.form-textarea {
  width: 100%;
  height: 150px; /* ความสูงของ textarea */
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #3a3a3a; /* สีพื้นหลังของ textarea */
  color: #ffffff; /* สีตัวอักษรใน textarea */
  resize: none; /* ปิดการปรับขนาด */
}

.thumbnail-preview {
  margin-bottom: 15px;
}

.thumbnail-img {
  max-width: 100%;
  border-radius: 8px;
}

.btn-submit {
  background-color: #4caf50; /* สีสำหรับปุ่มสร้างบล็อก */
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.btn-cancel {
  background-color: #f44336; /* สีสำหรับปุ่มกลับ */
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.dropbox {
  outline: 2px dashed grey;
  background: #555; /* สีพื้นหลัง */
  padding: 10px;
  min-height: 100px; /* ความสูงขั้นต่ำ */
  position: relative;
  cursor: pointer;
  text-align: center;
}

.dropbox:hover {
  background: #777; /* สีพื้นหลังเมื่อเมาส์ชี้ */
}
</style>
