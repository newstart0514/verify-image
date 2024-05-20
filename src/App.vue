<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <el-button type="primary" round @click="handleOpen">点我打开弹窗</el-button>
  <!-- 
    @success: 成功的回调函数 
    @fail: 失败的回调函数
    @close: ** 必传，请注意需要 showDialog.value = false **;
    imageList：图片列表，可以自定义图片进行随机展示，需要传入自定义图片列表，不传为使用随机图片
    title：弹窗的标题
    content：弹窗内容
    chanelWidth: 滑动轨道的长度
    width: dialog的长度
    imgSize：图片长度
   -->
  <verify-dialog
    v-model="showDialog"
    @success="handleSuccess" 
    @fail="handleFail"
    @close="handleClose" 
    :imageList="imageList" 
    title="我是标题"
    content="我是内容"
    :chanelWidth="80"
    :width="400"
    :imgSize="100"
    ></verify-dialog>
</template>

<script setup>
import { ref } from 'vue'
import verifyDialog from './components/verifyDialog.vue'

const showDialog = ref(false);

const imageList = ref([
  'https://s11.ax1x.com/2023/03/22/ppd0aOx.jpg',
  'https://s21.ax1x.com/2024/05/02/pkkQFsg.png'
])

const handleSuccess = (data) => {
  console.log('成功的回调', data);
}
const handleFail = (data) => {
  console.log('失败的回调', data);
}

const handleOpen = () => {
  showDialog.value = true;
}

const handleClose = (data) => {
  showDialog.value = false;
}

</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
