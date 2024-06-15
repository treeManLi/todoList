<template>
  <div class="statusBar" >
    <div class="time">
      <span>9:42</span>
    </div>
    <div class="right">
      <span class="icon" aria-label="cellular">
        <img src="@/assets/images/cellular.svg">
      </span>
      <span class="icon" aria-label="wifi">
        <img src="@/assets/images/wifi.svg">
      </span>
      <span class="icon" aria-label="battery">
        <img src="@/assets/images/battery.svg">
      </span>
    </div>
  </div>
  <!-- 日历组件 -->
  <div v-show="showCalendar" class="calendar-container">
    <el-calendar :range="[new Date(2024, 4,5 ), new Date(2024, 4, 30)]"  class="custom-calendar" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const showCalendar = ref(false);
const startY = ref(0);
const endY = ref(0);





const handleTouchStart = (event) => {
  startY.value = event.touches[0].clientY;
};

const handleTouchMove = (event) => {
  endY.value = event.touches[0].clientY;
};

const handleTouchEnd = () => {
  const swipeDistance = endY.value - startY.value;
  if (swipeDistance > 50) {
    // 显示日历
    showCalendar.value = true;
  } else {
    // 隐藏日历
    showCalendar.value = false;
  }
};

onMounted(() => {
  window.addEventListener('touchstart', handleTouchStart);
  window.addEventListener('touchmove', handleTouchMove);
  window.addEventListener('touchend', handleTouchEnd);
});

onUnmounted(() => {
  window.removeEventListener('touchstart', handleTouchStart);
  window.removeEventListener('touchmove', handleTouchMove);
  window.removeEventListener('touchend', handleTouchEnd);
});
</script>

<style scoped>
.statusBar {
  position: fixed;
  display: flex;
  left: 0;
  top: 0;
  width: 100%;
  height: 88px;
  opacity: 1;
  z-index: 10; 
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
}

.time {
  margin-top: 12px;
  margin-left: 30px;
}

.time span {
  font-size: 14px;
  font-weight: 600;
  text-align: center;
}

.right {
  flex-wrap: nowrap;
  margin-top: 10px;
  margin-left: auto;
  display: flex;
  gap: 10px;
}

.icon {
  position: relative;
  right: 20px;
  width: 10px;
  height: 10px;
  margin-left: 10px;
}
.icon img{
  width: 20px;
  height: 20px;
}

.calendar-container {
  position: absolute;
  top: 220px; /* 确保日历在状态栏下面 */
  left: 0;
  width: 100%;
  height: 500px;
  padding: 10px; /* 添加内边距 */
  z-index: 8; /* 确保日历在状态栏下层 */
  background: white;
  box-shadow: 0px 4px 20px rgba(0, 0, 0, 0.15); /* 更柔和的阴影 */
  border-radius: 10px; /* 圆角边框 */
  overflow: hidden;
}
/* 隐藏 el-calendar 头部 */
.custom-calendar >>> .el-calendar__header {
  display: none;
}

.custom-calendar >>> .el-calendar-table td {
  border: none; /* 移除单元格边框 */
  padding: 15px; /* 调整单元格内边距 */
  font-size: 20px;
 
}

.custom-calendar >>> .el-calendar-table th {
  color: #888; /* 星期文字颜色 */
  padding: 10px; /* 星期单元格内边距 */
}

</style>
