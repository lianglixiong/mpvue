<template>
  <div>
    <!--pages/mine/mine.wxml-->
    <!--pages/shopCart/shopCart.wxml-->
<!-- 小程序购物车demo -->

    <view class="body">
      <!-- <>购物车列表 -->
      <view class="carts-list">
        <view v-for="(item, index) in carts" class="carts-item" :data-title="item.title" :data-url="item.url" bindtap="bindViewTap">
          <!-- //复选框 -->
          <view class="carts-radio">
            <icon v-if="item.selected" type="success_circle" size="20" bindtap="bindCheckbox" :data-index="index" />
            <icon v-else type="circle" size="20" bindtap="bindCheckbox" :data-index="index" />
          </view>
          <!-- //商品信息 -->
          <view class="carts-cnt">
            <image class="carts-image" :src="item.image" mode="aspectFill" />
            <view class="carts-info">
              <view class="carts-title clamp2">{{item.title}}</view>
              <view class="carts-subtitle">
                <text class="carts-price c--f60">￥{{item.price}}</text>
              </view>
            </view>
          </view>
          <!-- //数量加减 -->
          <view class="carts-num">
            <text class="minus" :class="minusStatuses[index]" :data-index="index" data-type="-" bindtap="bindCartNum">-</text>
            <input type="number" :data-index="index" bindinput="bindIptCartNum" :value="item.num" />
            <text class="plus normal" :data-index="index" data-type="+" bindtap="bindCartNum">+</text>
          </view>
          <!-- //删除 -->
          <icon class="carts-del" type="clear" size="16" color="#ccc" :data-index="index" bindtap="bindCartsDel" />
        </view>
      </view>

      <!-- //加载提示 -->
      <block v-if="showLoading">
        <view class="page-loading">
          <text class="weui-loading" />
          <text class="loading-text">加载中</text>
        </view>
      </block>

      <view class="carts-footer">
        <view class="bottomfixed">
          <view class="inner">
            <view class="chkAll" bindtap="bindSelectAll">
              <icon v-if="selectedAllStatus" type="success_circle" size="20" />
              <icon v:else type="circle" size="20" />
              <text>全选</text>
            </view>
            <view class="total">合计：￥{{totalPrice}}</view>
            <view v-if="selectedNum != 0" class="btn-pay">去结算({{selectedNum}})</view>
            <view v-else class="btn-pay disabled">去结算({{selectedNum}})</view>
          </view>
        </view>
      </view>
    </view>
  </div>
</template>

<script>
// import { formatTime } from '@/utils/index'
// import card from '@/components/card'

export default {
  components: {
    // card
  },

  data () {
    return {
      logs: [],
      userInfo: {},
      motto: 'Hello World',
      minusStatuses:[1,2,3,4],
      showLoading:false,
      // orderItems
      carts: [
        {
          cid: 0,
          title: "东阿阿胶",
          url: "http://img.800pharm.com/images/20181102/20181102142521_967.jpg",
          image: "http://img.800pharm.com/images/20181102/20181102142521_967.jpg",
          num: "2",
          price:"819",
          subtotal:"1638",
          selected:false
        },
        {
          cid: 1,
          title: "【润众】恩替卡韦分散片*28片装",
          url: "http://img.800pharm.com/images/20181102/20181102142641_560.jpg",
          image: "http://img.800pharm.com/images/20181102/20181102142641_560.jpg",
          num: "1",
          price:"340",
          subtotal:"340",
          selected:false
        },
        {
          cid: 2,
          title: "复方阿胶浆(无蔗糖)",
          url: "http://img.800pharm.com/images/20181102/20181102142734_852.jpg",
          image: "http://img.800pharm.com/images/20181102/20181102142734_852.jpg",
          num: "1",
          price:"268",
          subtotal:"268",
          selected:false
        },
        {
          cid: 3,
          title: "龟龄集",
          url: "http://img.800pharm.com/images/20181102/20181102143036_510.jpg",
          image: "http://img.800pharm.com/images/20181102/20181102143036_510.jpg",
          num: "1",
          price:"255",
          subtotal:"255",
          selected:false
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
    //const logs = (wx.getStorageSync('logs') || [])
    //this.logs = logs.map(log => formatTime(new Date(log)))

    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style>
/* …… 小程序购物车 {{{ …… */
.c--f60{color: #f60;}
.clamp1, .clamp2, .clamp3{display: -webkit-box!important; overflow: hidden; -webkit-box-orient: vertical;}
.clamp1{-webkit-line-clamp:1;}
.clamp2{-webkit-line-clamp:2;}
.clamp3{-webkit-line-clamp:3;}

.carts-list{padding: 0 20rpx;}
.carts-list .carts-item{border-top: 1rpx solid #e3e3e3; box-sizing: border-box; display: flex; align-items: center; justify-content: space-between; padding: 20rpx 10rpx; width: 100%; position: relative;}
.carts-list .carts-item:nth-child(1){border-top: 0;}
.carts-item .carts-cnt{flex: 1; padding: 0 20rpx;}
.carts-cnt .carts-image{float:left; margin-right: 20rpx; height: 150rpx; width: 150rpx;}
.carts-cnt .carts-info{display: flex; flex-direction: column; justify-content: space-between; min-height: 150rpx;}
.carts-cnt .carts-title{font-size: 28rpx;}
.carts-cnt .carts-subtitle{display: flex; flex-direction: row; font-size: 28rpx;}
.carts-list .carts-item .carts-del{position: absolute; top: 20rpx; right: 10rpx;}

/*数量加减*/
.carts-num{border: 1rpx solid #aaa; border-radius: 5rpx; display: flex; align-items: center; align-self: flex-end; text-align: center; height: 50rpx;}
.carts-num input{border-left: 1rpx solid #aaa; border-right: 1rpx solid #aaa; font-size: 30rpx; font-family: arial; height: 50rpx; line-height: 50rpx; width: 80rpx;}
.carts-num .minus, .carts-num .plus{color: #000; text-align: center; line-height: 50rpx; width: 40rpx;}
.carts-num .minus{font-size: 30rpx;}
.carts-num .minus.disabled{color: #ccc;}
.carts-num .plus{font-size: 34rpx;}
</style>
