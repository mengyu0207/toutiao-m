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
    <van-form @submit="login" ref="form" class="loginForm">
      <van-field
        v-model="mobile"
        name="mobile"
        placeholder="请输入手机号"
        :rules="mobileRules"
      >
        <template #label>
          <span class="toutiao toutiao-shouji"></span>
        </template>
      </van-field>
      <van-field
        class="form"
        v-model="code"
        type="text"
        name="code"
        placeholder="请输入验证码"
        :rules="codeRules"
      >
        <template #label>
          <span class="toutiao toutiao-yanzhengma"></span>
        </template>
        <template #right-icon>
          <van-count-down
            :time="1000 * 3"
            v-if="isShowDown"
            format="ss s"
            @finish="isShowDown = false"
          />
          <van-button v-else class="CodeBtn" round size="mini" @click="SendCode"
            >发送验证码</van-button
          >
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
import { login, sendCode } from '@/api/user'
import { mobileRules, codeRules } from './rules'

export default {
  name: 'LoginPage',
  components: {},
  props: {},
  data() {
    return {
      username: '',
      password: '',
      mobile: '',
      code: '',
      mobileRules,
      codeRules,
      isShowDown: false
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
      //
      this.$toast.loading({
        message: '正在加载，不要着急哦~',
        forbidClick: true
      })
      try {
        const res = await login(this.mobile, this.code)
        console.log(res)
        this.$store.commit('setUser', res.data.data)
        this.$router.push('/')
        this.$toast.success('登录成功')
      } catch (error) {
        console.log(error)

        this.$toast.fail('登录失败')
        const status = error.response.status
        switch (status) {
          case 400:
            this.$toast.fail(error.response.data.message)
            break
          case 507:
            this.$toast.fail('登录错误，请刷新')
            break
          default:
            this.$toast.fail('登录错误，请刷新')
            break
        }
      }
    },
    async SendCode() {
      try {
        await this.$refs.form.validate('mobile')
        await sendCode(this.mobile)
        this.isShowDown = true
      } catch (error) {
        if (!error.response) {
          this.$toast.fail('手机号格式不正确')
        } else {
          const status = error.response.status
          if (status === 404 || status === 429) {
            this.$toast.fail('error.response.data.message')
          }
        }
      }
    },
    increment() {
      this.$store.commit('setNumber', 6666)
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
.loginForm {
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
