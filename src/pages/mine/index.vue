<template>
  <div>
    <!--pages/mine/mine.wxml-->
    <view class="container">
     
      <view class="userinfo">
        <image class="userinfo-avatar" :src="userInfo.avatarUrl" background-size="cover"></image>
        <text class="userinfo-nickname">{{userInfo.nickName}}</text>
        <image src="../../images/person/account_bg.png" class="account-bg">
        </image>
      </view>
     
      <view class="separate"></view>
     
      <view class="order" catchtap="toOrder">
        <text class="myorder-text">我的订单</text>
        <text class="myorderlook-text">查看全部订单</text>
        <image class="next-image" src="/static/images/next.png"></image>
      </view>
      <view class="line"></view>
     
      <view class="navs">
        <block v-for="(item, index) in orderItems"  :key="name">
          <view class="nav-item" catchtap="toOrder" :data-type="item.name" :data-typeid="item.typeId">
            <image :src="item.imageurl" class="nav-image" />
            <text>{{item.name}}</text>
          </view>
        </block>
      </view>
     
      <view class="separate"></view>
      <view class="person-list">
        <view class="list-item">
          <image class="item-image" src="/static/images/personal_favorite.png"></image>
          <text class="item-text">我的收藏</text>
        </view>
        <view class="person-line"></view>
        <view class="list-item">
          <image class="item-image" src="/static/images/personal_site.png"></image>
          <text class="item-text">收货地址</text>
        </view>
        <view class="person-line"></view>
        <view class="list-item">
          <image class="item-image" src="/static/images/personal_sale_record.png"></image>
          <text class="item-text">售后记录</text>
        </view>
        <view class="person-line"></view>
        <view class="list-item">
          <image class="item-image" src="/static/images/personal_evaluated.png"></image>
          <text class="item-text">我的评价</text>
        </view>
        <view class="person-line"></view>
        <view class="list-item">
          <image class="item-image" src="/static/images/personal_share.png"></image>
          <text class="item-text">推广邀请</text>
        </view>
      </view>
      <view class="separate"></view>
    </view>
  </div>
</template>

<script>
import { formatTime } from '@/utils/index'
import card from '@/components/card'

export default {
  components: {
    card
  },

  data () {
    return {
      logs: [],
      userInfo: {},
      motto: 'Hello World',
      // orderItems
      orderItems: [
        {
          typeId: 0,
          name: '待付款',
          url: 'bill',
          imageurl: '../../images/person/personal_pay.png',
        },
        {
          typeId: 1,
          name: '待发货',
          url: 'bill',
          imageurl: '../../images/person/personal_shipped.png',
        },
        {
          typeId: 2,
          name: '待收货',
          url: 'bill',
          imageurl: '../../images/person/personal_receipt.png'
        },
        {
          typeId: 3,
          name: '待评价',
          url: 'bill',
          imageurl: '../../images/person/personal_comment.png'
        }
      ],

    }
  },

  methods:{
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
  },

  created () {
    const logs = (wx.getStorageSync('logs') || [])
    this.logs = logs.map(log => formatTime(new Date(log)))

    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style>
/* pages/mine/mine.wxss */
.userinfo {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #f0145a;
}
.account-bg {
  width: 100%;
  height: 150rpx;
}
.userinfo-avatar {
  width: 108rpx;
  height: 108rpx;
  margin: 20rpx;
  border-radius: 50%;
}
.userinfo-nickname {
  color: #fff;
}
/* 订单 */
.order {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  height: 90rpx;
}
.myorder-text {
  font-size: 30rpx;
  color: gray;
  margin: 20rpx;
  width: 40%;
}
.myorderlook-text {
  font-size: 30rpx;
  color: gray;
  position: relative;
  right: 20rpx;
  width: 60%;
  text-align: right;
}
.next-image {
  width: 60rpx;
  height: 60rpx;
  position: relative;
  right: 10rpx;
}
.navs {
  display: flex;
}
.nav-item {
  width: 25%;
  display: flex;
  align-items: center;
  flex-direction: column;
  padding: 20rpx;
}
.nav-item .nav-image {
  width: 40rpx;
  height: 40rpx;
  margin: 5rpx;
}
.nav-item text {
  margin-top: 20rpx;
  font-size: 25rpx;
  color: gray;
}
/* 列表 */
.person-list {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: left;
}
.list-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 80rpx;
}
.item-image {
  width: 40rpx;
  height: 40rpx;
  margin: 20rpx;
}
.item-text {
  color: gray;
  font-size: 25rpx;
  margin-left: 20rpx;
}
.person-line {
  width: 100%;
  height: 2rpx;
  background: lightgray;
  margin-left: 90rpx;
}
</style>
