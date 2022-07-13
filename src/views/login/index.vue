<template>
  <div class="login-container">
    <van-nav-bar
      class="navbar"
      title="标题"
      left-arrow
      @click-left="BackToPrepage"
    >
      <template #left><van-icon name="cross" /></template>
    </van-nav-bar>
    <van-form @submit="login" class="form">
      <van-field
        v-model="mobile"
        name="用户名"
        placeholder="请输入手机号"
        :rules="[{ required: true, message: '请填写用户名' }]"
      >
        <template #label>
          <span class="toutiao toutiao-shouji"></span>
        </template>
      </van-field>
      <van-field
        class="form"
        v-model="code"
        type="text"
        name="密码"
        placeholder="请输入验证码"
        :rules="[{ required: true, message: '请填写密码' }]"
      >
        <template #label>
          <span class="toutiao toutiao-yanzhengma"></span>
        </template>
        <template #right-icon>
          <van-button class="CodeBtn" round size="mini">发送验证码</van-button>
        </template>
      </van-field>
      <div style="margin: 16px">
        <van-button block type="info" native-type="submit">登录</van-button>
      </div>
    </van-form>
  </div>
</template>

<script>
import { Toast } from 'vant'
import { login } from '@/api/user'
export default {
  name: 'LoginPage',
  components: {},
  props: {},
  data() {
    return {
      username: '',
      password: '',
      mobile: '',
      code: ''
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {
    onClickLeft() {
      Toast('宇哥威武')
    },
    BackToPrepage() {
      this.$route.back()
    },
    onSubmit(values) {
      console.log('submit', values)
    },
    async login() {
      const res = await login(this.mobile, this.code)
      console.log(res)
    }
  }
}
</script>
<style scoped lang="less">
.navbar {
  background-color: pink;
  :deep(.van-nav-bar__title) {
    color: yellow;
  }
}
.form {
  :deep(.van-field__label) {
    flex: 1;
  }
  :deep(.van-cell__value) {
    flex: 20;
  }
  .toutiao {
    font-size: 0.37333rem;
  }
  .CodeBtn {
    background-color: #fff;
    color: pink;
  }
}
</style>
