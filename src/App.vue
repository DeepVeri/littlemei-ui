<template>
  <div class="app">
    <Header />
    <div class="main-container">
      <Sidebar />
      <main class="main-content">
        <router-view />
      </main>
    </div>
    <Footer />
    <FloatingLangSwitcher />
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useLanguage } from './composables/useLanguage'
import Header from './components/Header.vue'
import Sidebar from './components/Sidebar.vue'
import Footer from './components/Footer.vue'
import FloatingLangSwitcher from './components/FloatingLangSwitcher.vue'

// 初始化语言设置
useLanguage()

onMounted(() => {
  // 设置页面标题
  document.title = 'LittleMei - 女装时尚购物平台'
})
</script>

<style scoped>
.app {
  width: 100%;
  background: #fafafa;
  min-height: 100vh;
}

.main-container {
  display: flex;
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  gap: 30px;
}

.main-content {
  flex: 1;
  min-width: 0;
}

/* 平板端 */
@media (max-width: 1024px) {
  .main-container {
    padding: 15px;
    gap: 20px;
  }
}

/* 移动端 */
@media (max-width: 768px) {
  .main-container {
    flex-direction: column;
    padding: 10px;
    gap: 0;
  }

  .main-content {
    padding: 0;
  }
}

/* 小屏手机 */
@media (max-width: 480px) {
  .app {
    padding-bottom: 80px; /* 为浮动按钮留出空间 */
  }

  .main-container {
    padding: 8px;
  }
}

/* 超小屏幕 */
@media (max-width: 360px) {
  .main-container {
    padding: 6px;
  }
}
</style>

