<template>
  <div>
    <!-- 头部 -->
    <header>
      <!-- 登录的盒子 -->
      <div v-if="isLogin">用户信息</div>
      <!-- 未登录 -->
      <div v-else>登录和注册</div>
    </header>
    <!-- 主体 -->
    <main>主体</main>
    <!-- 退出按钮 -->
    <van-button v-if="isLogin">退出按钮</van-button>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  computed: {
    isLogin() {
      return !!this.$store.state.user.token
    }
  }
}
</script>

<style></style>
