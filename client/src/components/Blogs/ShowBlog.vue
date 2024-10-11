<template>
    <div class="blog-detail-container">
        <div class="blog-detail">
            <p><strong>ID:</strong> {{ blog.id }}</p>
            <p><strong>ชื่ออาหาร:</strong> {{ blog.title }}</p>
            <p><strong>ส่วนผสม:</strong> {{ blog.category }}</p>
            <p><strong>ขั้นตอนการทำ:</strong> {{ blog.status }}</p>
        </div>
        <div class="blog-actions">
            <button class="btn-edit" v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไขบล็อก</button>
            <button class="btn-back" v-on:click="navigateTo('/blogs')">กลับ</button>
        </div>
    </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'
export default {
    data () {
        return {
            blog: null
        }
    },
    async created () {
        try {
            let blogId = this.$route.params.blogId
            this.blog = (await BlogsService.show(blogId)).data
        } catch (error) {
            console.log(error)
        }
    },
    methods: {
        navigateTo (route) {
            this.$router.push(route)
        },
    }
}
</script>

<style scoped>
/* การจัดการ container */
.blog-detail-container {
    max-width: 800px;
    margin: 30px auto;
    background: #f9f9f9;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
}

/* การจัดการรายละเอียดบล็อก */
.blog-detail {
    margin-bottom: 20px;
    text-align: left;
}

.blog-detail p {
    margin: 10px 0;
    font-size: 16px;
    line-height: 1.5;
}

/* การจัดการปุ่ม */
.blog-actions {
    display: flex;
    justify-content: center;
    gap: 10px;
}

.btn-edit, .btn-back {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    color: white;
}

/* สไตล์ปุ่มสำหรับการแก้ไขและกลับ */
.btn-edit {
    background-color: #4CAF50;
}

.btn-back {
    background-color: #f44336;
}

/* เอฟเฟกต์เมาส์ชี้ */
.btn-edit:hover, .btn-back:hover {
    opacity: 0.8;
}
</style>
