<template>
  <div class="notification-content">
    <p>
      <img :src="imageSrc" alt="Example Image" class="notification-image" />
    </p>
    <p v-if="guide" style="text-indent: 2em;">
      {{ guide.instructions }}
    </p>
    <p v-else style="text-align: center;">
      正在加载器械操作指南...
    </p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const imageSrc = ref('')
const guide = ref(null)

const fetchGuide = async () => {
  try {
    const response = await axios.get('/api/AIGuide/GetEquipmentGuide', {
      params: { equipmentName: '椭圆机' } // 在此处替换为你需要查询的器械名称
    })
    guide.value = response.data.guide
    imageSrc.value = guide.value.imgUrl // 设置图片链接
  } catch (error) {
    console.error('Failed to fetch guide:', error)
  }
}
</script>

<style scoped>
.notification-content {
  text-align: left;
  padding: 20px;
  /* 添加一些内边距 */
}

.notification-image {
  width: 70%;
  max-height: 400px;
  /* 设置最大高度 */
  object-fit: cover;
  /* 确保图片按比例缩放 */
  margin: 20px auto;
  /* 确保图片水平居中 */
  display: block;
  /* 确保图片作为块级元素 */
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
</style>