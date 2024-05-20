<template>
  <el-dialog
    v-model="show"
    :title="props.title"
    :width="props.width"
    center
    close="handleClose"
  >
    <div class="container">
      <div style="margin-bottom: 1rem;font-weight: 600;">{{ props.content }}</div>
      <div>
        <el-avatar :size="props.imgSize" :src="imageUrl" :style="{ transform: `rotate(${angle + sliderValue}deg)` }" />
      </div>
    </div>
    <template #footer>
      <div :style="{ width: `${props.chanelWidth}%`, marginLeft: `${excludeDistance}%` }">
        <el-slider v-model="sliderValue" :show-tooltip="false" :min="0" :max="360" @change="handleChange" />
      </div>
    </template>
  </el-dialog>
</template>

<script setup>
import { onMounted, ref, defineProps, defineEmits  } from 'vue'

const props = defineProps({
  value: {
    type: Boolean,
    default: false,
  },
  imageList: {
    type: Array,
    default: []
  },
  title: {
    type: String,
    default: '人机验证'
  },
  chanelWidth: {
    type: Number,
    default: 70
  },
  width: {
    type: Number,
    default: 500
  },
  imgSize: {
    type: Number,
    default: 150
  },
  content: {
    type: String,
    default: '请将图片旋转至正确向上'
  }
});
const emit = defineEmits();

const show = ref(true);
const sliderValue = ref(0)
const angle = ref(0)
const imageUrl = ref('')
const excludeDistance = ref(0);

const handleChange = (value) => {
  const sum = sliderValue.value + angle.value
  // 设置为5，容许用户有10度的偏差，防止需要精确到具体
  if (sum % 360 < 10 || sum % 360 > 350) {
    console.log('验证成功')
    emit('success', '验证成功')
  } else {
    console.log('验证失败')
    emit('fail', '验证失败')
  }
  sliderValue.value = 0
}
const getRandomStringFromArray = (stringArray) => {
  if (!Array.isArray(stringArray)) {
    console.error('字符串数组非法')
    return 'https://picsum.photos/200/300'
  }
  if (stringArray.length === 0) {
    // 没有传递数组，使用随机图片生成
    return 'https://picsum.photos/200/300'
  }
  const randomIndex = Math.floor(Math.random() * stringArray.length);
  return stringArray[randomIndex]
}
const handleClose = () => {
  emit('close', '关闭弹窗')
}

onMounted(() => {
  angle.value = Math.floor(Math.random() * 360)
  imageUrl.value = getRandomStringFromArray(props.imageList);
  excludeDistance.value = Math.floor((100 - props.chanelWidth) / 2);
})
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>