<template>
  <div class="container" @click="clickHandle('test click', $event)">
    <swiper :images="images" />

    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>

    <div class="usermotto">
      <div class="user-motto">
        <card :text="motto"></card>
      </div>
    </div>

    <form class="form-container">
      <input type="text" class="form-control" v-model="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />
    </form>
    <a href="/pages/counter/main" class="counter">去往Vuex示例页面</a>
    <!-- <div class="footer">
      <ul>
        <li>首页</li>
        <li>分类</li>
        <li>购物车</li>
        <li>我的</li>
      </ul>
    </div> -->
  </div>
</template>

<script>

import card from '@/components/card'
import swiper from "@/components/swiper"

export default {
  data () {
    return {
      motto: 'Hello World',
      userInfo: {},
      images: [{
        url:"https://img-oss.yunshanmeicai.com/goods/default/31d8dfa4-0d7b-4694-80f9-41b07c9d0a3a.png"
      },{
        url:"https://img-oss.yunshanmeicai.com/goods/default/e83c8f0f-4acc-4729-bcbb-294f2b314977.jpg"
      }]
    }
  },

  components: {
    card,
    swiper
  },

  methods: {
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style scoped lang=scss>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
.footer{
  width: 100%;
  height: 80rpx;
  position: fixed;
  bottom: 0;
  left: 0;
  ul{
    display: flex;
    justify-content: space-between;
    height: 100%;
    li{
      flex: 1;
      text-align: center;
      line-height: 80rpx;
    }
  }
}
</style>
