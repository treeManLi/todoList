<template>
  <!-- 菜单盒子 -->
  <div v-show="showMenu" class="menu-box" >
    <div class="menu-item">
      <img src="@/assets/images/history.svg" alt="历史 Icon" class="menu-icon" />
      <span>历史</span>
    </div>
    <div class="menu-item">
      <img src="@/assets/images/list.svg" alt="列表 Icon" class="menu-icon" />
      <span>列表</span>
    </div>
    <div class="menu-item">
      <img src="@/assets/images/color-tag.svg" alt="颜色标签 Icon" class="menu-icon" />
      <span>颜色标签</span>
    </div>
    <div class="menu-item">
      <img src="@/assets/images/trash.svg" alt="回收站 Icon" class="menu-icon" />
      <span>回收站</span>
    </div>
    <div class="menu-item">
      <img src="@/assets/images/setting.svg" alt="设置 Icon" class="menu-icon" />
      <span>设置</span>
    </div>
  </div>

  <!-- 个人中心盒子 -->
  <div v-show="showProfile" class="profile-box">
    <img src="@/assets/images/user.png" alt="Avatar" class="avatar" />
    <span class="nickname">卓君</span>
    <button class="profile-button">服务与协议</button>
    <button class="profile-button">用户隐私</button>
    <button class="profile-button">清理缓存</button>
    <button class="profile-button">登出账号</button>

    <div class="vip">
      <span class="text">VIP</span>
      <span>订阅更高版本</span>
      <span><img src="@/assets/images/arrow.png" class="arrow"></span>
    </div>
  </div>

  <!-- 底部 -->
  <div class="footer" >
    <button class="footer-button" @click="toggleMenu">
      <img src="@/assets/images/menu.svg" alt="Menu Icon" class="footer-icon">
    </button>
    <button class="footer-button" >
      <img src="@/assets/images/theme.svg" alt="Theme Icon" class="footer-icon">
    </button>
    <button class="footer-button" @click="toggleProfile">
      <img src="@/assets/images/user.png" alt="User Icon" class="footer-icon">
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const showCalendar = ref(false);
const showMenu = ref(false);
const showProfile = ref(false);


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
    showCalendar.value = true;
  } else {
    showCalendar.value = false;
  }
};

const toggleMenu = () => {
  showMenu.value = !showMenu.value;
  showProfile.value = false;
};

const toggleProfile = () => {
  showProfile.value = !showProfile.value;
  showMenu.value = false;
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
  z-index: 10; /* 确保状态栏在日历上层 */
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1); /* 添加阴影 */
}

.calendar-container {
  position: absolute;
  top: 100px; /* 确保日历在状态栏下面 */
  left: 20px;
  height: 50px;
  width: 100%;
  z-index: 10; /* 确保日历在状态栏下层 */
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1); /* 添加阴影 */
  background: white;
}

.custom-calendar {
  --el-calendar-header-font-size: 14px;
  --el-calendar-cell-height: 40px;
}

/* 隐藏 el-calendar 头部 */
.custom-calendar >>> .el-calendar__header {
  display: none;
}

.menu-box {
  position: absolute;
  bottom: 70px; /* 确保在footer上方 */
  left: 10px;
  right: 10px;
  padding: 20px;
  background: white;
  box-shadow: 0px 4px 20px rgba(0, 0, 0, 0.15);
  border-radius: 10px;
  height: 300px; /* 根据需要调整高度 */
  z-index: 11;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
}
.profile-box {
  position: absolute;
  bottom: 70px; /* 确保在footer上方 */
  left: 10px;
  right: 10px;
  padding: 20px;
  background: white;
  box-shadow: 0px 4px 20px rgba(0, 0, 0, 0.15);
  border-radius: 10px;
  height: 700px; /* 根据需要调整高度 */
  z-index: 11;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 10px;
  width: 100%;
  cursor: pointer;
}

.menu-item span {
  margin-left: 10px;
}

.menu-icon {
  width: 24px;
  height: 24px;
}

.profile-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-bottom: 20px;
}

.nickname {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 20px;
}

.profile-button {
  width: 80%;
  padding: 10px;
  margin: 5px 0;
  font-size: 17px;
  font-weight: 650;
  background: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  text-align: center;
}

.vip{
  display: flex;
  width: 300px;
  margin-top: 50px;
  justify-content: space-between;
  height: 64px;
  opacity: 1;
  border-radius: 8px;
  background: rgba(255, 255, 255, 1);
  box-shadow: 0px 4px 12px -4px rgba(0, 0, 0, 0.08);
  font-size: 17px;
  font-weight: 500;
  letter-spacing: 0px;
  line-height: 21.72px;
  color: rgba(0, 122, 255, 1);
}
.arrow{
  width: 20px;
  height: 20px;
}
.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 10px 20px;
  background: white;
  box-shadow: 0px -2px 10px rgba(0, 0, 0, 0.1);
}

.footer-button {
  background: none;
  border: none;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}

.footer-button:focus {
  outline: none;
}

.footer-icon {
  width: 24px; /* 根据实际需要调整图标的大小 */
  height: auto;
}
/* 亮模式样式 */
.light-theme {
  background-color: #f0f0f0;
  color: #333;
}

/* 暗模式样式 */
.dark-theme {
  background-color: #333;
  color: #f0f0f0;
}
</style>
