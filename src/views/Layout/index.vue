<template>
  <div>
    <router-view></router-view>
    <van-tabbar route>
      <van-tabbar-item name="home" icon="home-o" to="/ ">首页</van-tabbar-item>
      <van-tabbar-item name="search" icon="play-circle-o" to="/video"
        >视频</van-tabbar-item
      >
      <van-tabbar-item name="friends" icon="chat-o" to="/qa"
        >问答</van-tabbar-item
      >
      <van-tabbar-item name="setting" icon="manager-o" to="/my">{{
        !!this.$store.state.user.token ? '我的' : '未登录'
      }}</van-tabbar-item>
    </van-tabbar>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style></style>
