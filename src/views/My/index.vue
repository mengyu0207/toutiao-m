<template>
  <div class="my">
    <!-- 头部 -->
    <header>
      <!-- 登录的盒子 -->
      <div v-if="isLogin" class="user-info banner">
        <!-- 占位 -->
        <van-row></van-row>
        <!-- 展示用户信息 -->
        <van-row class="row-2">
          <van-col span="12">
            <van-row type="flex" align="center" justify="space-around">
              <van-image
                round
                width="1.76rem"
                height="1.76rem"
                :src="userInfo.photo"
              />
              <span class="mobile">{{ userInfo.name }}</span>
            </van-row>
          </van-col>
          <van-col span="11">
            <van-row class="code-row" type="flex" align="center" justify="end">
              <van-button round class="code-btn" size="mini"
                >编辑资料</van-button
              >
            </van-row>
          </van-col>
        </van-row>
        <!-- 用户文章的数量 -->
        <van-row>
          <van-grid class="grid" :border="false">
            <van-grid-item text="头条">
              <template #icon>{{ userInfo.art_count }}</template>
            </van-grid-item>
            <van-grid-item text="粉丝">
              <template #icon>{{ userInfo.fans_count }}</template>
            </van-grid-item>
            <van-grid-item text="关注">
              <template #icon>{{ userInfo.follow_count }}</template>
            </van-grid-item>
            <van-grid-item text="获赞">
              <template #icon>{{ userInfo.like_count }}</template>
            </van-grid-item>
          </van-grid>
        </van-row>
      </div>
      <!-- 未登录 -->
      <!-- 未登录的头部盒子 -->
      <div v-else class="login-register banner">
        <div class="wrap" @click="goLogin">
          <img src="../../assets/mobile.png" />
          <span>登录/注册</span>
        </div>
      </div>
    </header>
    <!-- 主体 -->

    <main>
      <div>
        <van-grid column-num="2" class="grid" clickable>
          <van-grid-item text="收藏">
            <template #icon>
              <span class="toutiao toutiao-shoucang"></span>
            </template>
          </van-grid-item>
          <van-grid-item text="历史">
            <template #icon>
              <span class="toutiao toutiao-lishi"></span>
            </template>
          </van-grid-item>
        </van-grid>
      </div>

      <div class="link">
        <van-cell title="消息通知" is-link></van-cell>
        <van-cell title="宇哥" is-link></van-cell>
      </div>
    </main>

    <!-- 退出按钮 -->
    <van-button v-if="isLogin" block class="login-btn" @click="logout"
      >退出按钮</van-button
    >
  </div>
</template>

<script>
import { getUserInfo } from '@/api/user'
export default {
  data() {
    return {
      userInfo: {}
    }
  },
  created() {
    this.getUserInfo()
  },
  methods: {
    goLogin() {
      this.$router.push('/login')
    },
    logout() {
      this.$dialog
        .confirm({
          title: '黑马头条',
          message: '是否确认退出账号'
        })
        .then(() => {
          this.$store.commit('setUser', {})
        })
        .catch(() => {})
    },
    async getUserInfo() {
      if (this.isLogin) {
        try {
          const {
            data: { data }
          } = await getUserInfo()
          this.userInfo = data
        } catch (error) {
          this.$toast.fail('请重新登录')
        }
      }
    },
    computed: {
      isLogin() {
        return !!this.$store.state.user.token
      }
    }
  }
}
</script>

<style scoped lang="less">
.banner {
  width: 100%;
  height: 400px;
  background: url('../../assets/banner.png') no-repeat 0 0 / cover;
}
.user-info {
  display: flex;
  flex-direction: column;

  > .van-row {
    flex: 1;
  }
  .row-2 {
    .van-col {
      height: 100%;
    }
  }
  .mobile {
    font-size: 0.4rem;
    color: #fff;
  }
}
.code-btn {
  width: 1.53333rem;
  height: 0.42667rem;
  background: #fff;
  border-radius: 0.21333rem;
  font-size: 0.26667rem;
  color: #666;
  padding: 0;
}

.code-row {
  height: 100%;
}
.grid {
  :deep(.van-grid-item__content) {
    background-color: unset;
  }
}
.van-grid-item {
  color: #fff;
  :deep(.van-grid-item__text) {
    color: #fff;
  }
}
// 主体区域
main {
  .van-grid-item {
    color: #fff;
    :deep(.van-grid-item__text) {
      color: black;
      font-size: 25px;
    }
  }
  .grid {
    color: #646566;
    // 字体图标
    .toutiao {
      font-size: 45px;

      &.toutiao-lishi {
        color: #ffb31d;
      }
      &.toutiao-shoucang {
        color: #ed5253;
      }
    }
  }
}
.login-btn {
  :deep(.van-button-text) {
    color: red;
  }
}
.my {
  background-color: #f5f7f9;
  height: calc(100vh-100px);
}
.login-register {
  display: flex;
  justify-content: center;
  align-items: center;
  .wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #fff !important;
    img {
      width: 66px;
      height: 66px;

      margin-bottom: 10px;
    }
    span {
      font-size: 0.37rem;
    }
  }
}
</style>
