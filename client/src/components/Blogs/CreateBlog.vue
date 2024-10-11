<template>
  <div class="create-blog-container">
    <h1 class="header-title">แนะนำเมนูใหม่ได้เลย</h1>
    <form v-on:submit.prevent="createBlog" class="blog-form">
      <div class="form-group">
        <label for="title">ชื่ออาหาร:</label>
        <input type="text" id="title" v-model="blog.title" class="form-control" />
      </div>

      <transition name="fade">
        <div class="thumbnail-pic" v-if="blog.thumbnail !== 'null'">
          <img :src="BASE_URL + blog.thumbnail" alt="thumbnail" />
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
            @change="
              filesChange($event.target.name, $event.target.files);
              fileCount = $event.target.files.length;
            "
            accept="image/*"
            class="input-file"
          />
          <p v-if="isInitial">ลากไฟล์ที่นี่หรือคลิกเพื่อเลือกไฟล์</p>
          <p v-if="isSaving">กำลังอัปโหลด {{ fileCount }} ไฟล์...</p>
          <p v-if="isSuccess">อัปโหลดสำเร็จแล้ว</p>
        </div>
      </div>

      <div>
        <transition-group tag="ul" class="pictures">
          <li v-for="picture in pictures" v-bind:key="picture.id">
            <img :src="BASE_URL + picture.name" alt="picture image" />
            <br />
            <button v-on:click.prevent="useThumbnail(picture.name)">Thumbnail</button>
            <button v-on:click.prevent="delFile(picture)">ลบ</button>
          </li>
        </transition-group>
        <div class="clearfix"></div>
      </div>

      <div class="form-group">
        <label for="category">ส่วนผสม:</label>
        <textarea id="category" v-model="blog.category" class="form-textarea" placeholder="กรุณากรอกส่วนผสม"></textarea>
      </div>

      <div class="form-group">
        <label for="status">ขั้นตอนการทำ:</label>
        <textarea id="status" v-model="blog.status" class="form-textarea" placeholder="กรุณากรอกขั้นตอนการทำ"></textarea>
      </div>

      <div class="form-group">
        <label for="giver">ผู้ให้สูตร:</label>
        <input type="text" id="giver" v-model="blog.giver" class="form-control" placeholder="กรุณากรอกชื่อผู้ให้สูตร" />
      </div>

      <div class="form-actions">
        <button type="submit" class="btn-submit">สร้างบล็อก</button>
      </div>
    </form>
  </div>
</template>

<script>
import BlogsService from "@/services/BlogsService";
import UploadService from "../../services/UploadService";

export default {
  data() {
    return {
      BASE_URL: "http://localhost:8081/assets/uploads/",
      uploadFieldName: "userPhoto",
      fileCount: 0,
      blog: {
        title: "",
        thumbnail: "null",
        pictures: "null",
        content: "",
        category: "",
        status: "saved",
        giver: "", // ฟิลด์สำหรับชื่อผู้ให้สูตร
      },
    };
  },
  methods: {
    async delFile(material) {
      let result = confirm("ต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        let dataJSON = {
          filename: material.name,
        };

        await UploadService.delete(dataJSON);
        for (var i = 0; i < this.pictures.length; i++) {
          if (this.pictures[i].id === material.id) {
            this.pictures.splice(i, 1);
            this.materialIndex--;
            break;
          }
        }
      }
    },
    async createBlog() {
      this.blog.pictures = JSON.stringify(this.pictures);
      try {
        await BlogsService.post(this.blog);
        this.$router.push({ name: "blogs" });
      } catch (err) {
        console.log(err);
      }
    },
    filesChange(fieldName, fileList) {
      const formData = new FormData();
      if (!fileList.length) return;
      Array.from(fileList).forEach((file) => {
        formData.append(fieldName, file, file.name);
      });
      this.save(formData);
    },
    async save(formData) {
      // ... (logic to save the file)
    },
    useThumbnail(filename) {
      this.blog.thumbnail = filename;
    },
  },
};
</script>

<style scoped>
.create-blog-container {
  max-width: 800px;
  margin: auto;
  background: #2c2c2c; /* สีพื้นหลัง */
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: #ffffff; /* สีตัวอักษร */
  text-align: center;
}

.header-title {
  text-align: center; /* จัดกลางข้อความ */
  margin-bottom: 20px; /* เพิ่มระยะห่างใต้หัวข้อ */
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

.form-textarea {
  width: 100%;
  height: 150px; /* ปรับความสูงของ textarea */
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #3a3a3a; /* สีพื้นหลังของ textarea */
  color: #ffffff; /* สีตัวอักษรใน textarea */
  resize: none; /* ปิดการปรับขนาด */
}

.dropbox {
  outline: 2px dashed grey; /* เส้นขอบ */
  outline-offset: -10px;
  background: #555; /* สีพื้นหลัง */
  color: dimgray;
  padding: 10px;
  min-height: 100px; /* ความสูงขั้นต่ำ */
  position: relative;
  cursor: pointer;
  text-align: center;
}

.input-file {
  opacity: 0; /* ซ่อน input file */
  width: 100%;
  height: 100%;
  position: absolute;
  cursor: pointer;
}

.dropbox:hover {
  background: #777; /* สีพื้นหลังเมื่อเมาส์ชี้ */
}

.thumbnail-pic img {
  width: 200px;
}

.btn-submit {
  background-color: #4caf50; /* สีสำหรับปุ่มสร้างบล็อก */
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-submit:hover {
  background-color: #45a049; /* สีเมื่อเมาส์ชี้ */
}
</style>
