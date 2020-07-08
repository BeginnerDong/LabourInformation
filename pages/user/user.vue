<template>
	<view>
		<view class="bg-mcolor text-center">
			<image src="../../static/images/about-img.png" class="userImg m-a "></image>
			<view class="font-30 colorf pb-5 pt-3 line-h position-relative">
				蜗牛
				<!-- <text class="tag tagUser position-absolute">普通用户</text> -->
				<text class="tag tagUser position-absolute">高级用户</text>
			</view>
		</view>
		<!-- 高级用户显示 -->
		<view class="font-24 p-3 oh">
			桥隧之家是一个完全免费的信息发布平台，只要您符合条件就可以免费开通相关权限和认证。
			<text class="Mcolor">详情>></text>
		</view>
		<!-- 普通用户显示 -->
		<!-- <view class="font-28 color2 d-flex j-sb a-center py-4 px-3 bg-white mt-2">
			<view>我的二手交易信息主页</view>
			<view class="Mcolor" @click="changeHand()">免费开通</view>
		</view> -->
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white borderB-e1" @click="Router.redirectTo({route:{path:'/pages/user-secondHandIndex/user-secondHandIndex'}})">
			<view>我的二手交易信息主页</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="font-30 Mcolor py-4 px-3 bg-white mt-2 borderB-e1 d-flex a-center">
			<image src="../../static/images/about-icon.png" class="icon1"></image>
			<view class="pl-2">我发布的</view>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white borderB-e1" @click="Router.redirectTo({route:{path:'/pages/user-secondHand/user-secondHand'}})">
			<view>二手交易信息</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white borderB-e1" @click="Router.redirectTo({route:{path:'/pages/user-labour/user-labour'}})">
			<view>劳务招聘信息</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white" @click="Router.redirectTo({route:{path:'/pages/user-business/user-business'}})">
			<view>商务通信息</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white mt-2" @click="Router.redirectTo({route:{path:'/pages/user-collection/user-collection'}})">
			<view>我的收藏</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		
		<!-- ----------高级用户显示------------- -->
		<view>
			<view class="font-30 Mcolor py-4 px-3 bg-white mt-2 borderB-f5 d-flex a-center">
				<image src="../../static/images/about-icon1.png" class="icon3"></image>
				<view class="pl-2">用工不良记录</view>
			</view>
			<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white">
				<view class="d-flex a-center w-50 j-center" @click="Router.redirectTo({route:{path:'/pages/user-workersRecord/user-workersRecord'}})">
					<image src="../../static/images/about-icon2.png" class="icon4"></image>
					<view class="pl-2">我要登记</view>
				</view>
				<view class="d-flex a-center w-50 j-center" @click="Router.redirectTo({route:{path:'/pages/user-workersSreach/user-workersSreach'}})">
					<image src="../../static/images/about-icon3.png" class="icon5"></image>
					<view class="pl-2">我要查询</view>
				</view>
			</view>
		</view>
		<!-- --------------------- -->
		
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white mt-2 borderB-e1" @click="Router.redirectTo({route:{path:'/pages/user-opinion/user-opinion'}})">
			<view>意见反馈</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white" @click="Router.redirectTo({route:{path:'/pages/contentUs/contentUs'}})">
			<view>联系我们</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		
		
		<!-- 开通二手 -->
		<view class="bg-mask px-4" v-show="hand_show">
			<view class="font-28 color2 bg-white m-a text-center rounded10" style="margin-top: 370rpx;">
				<view class="pb-2 pt-5">免费开通二手交易主页，请联系微信客服</view>
				<view class="Rcolor pb-2">微信号：nhd55866</view>
				<view>加微信备注：开通主页</view>
				<view class="Mcolor font-30 mt-5 py-4 borderT-e1"  @click="changeHand()">确认</view>
			</view>
		</view>
		
		
		<view style="height: 150rpx;width: 100%;"></view>
		<!-- 底部 -->
		<view class="footer z-index1000"> 
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/secondHand/secondHand'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>二手</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/business/business'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>商务通</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/labour/labour'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>劳务</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar5-a.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				hand_show:false
			}
		},
		methods: {
			changeHand(){
				const self = this;
				self.hand_show = !self.hand_show
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.userImg{height: 140rpx;width: 140rpx;}
.tagUser{top: 30rpx;margin-left: 10rpx;}
.oh{color: #D4A255;background-color: #FFF4D6;}

.icon1{width: 30rpx;height: 30rpx;}
.icon2{width: 12rpx;height: 21rpx;}
.icon3{width: 34rpx;height: 30rpx;}
.icon4{width: 34rpx;height: 36rpx;}
.icon5{width: 31rpx;height: 34rpx;}
</style>
