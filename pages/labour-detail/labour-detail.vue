<template>
	<view>
		<!-- banner -->
		<view class="banner">
			<swiper class="swiper-box h-100" autoplay="autoplay" interval="4000">
				<block v-for="(item,index) in 3" :key="index">
					<swiper-item class="swiper-item">
						<image src="../../static/images/detailsl-img1.png" />
					</swiper-item>
				</block>
			</swiper>
			<view class="swiper-sign colorf font-20 line-h-md px-1 rounded10">1/5</view>
		</view>
		
		<view class="px-3 bg-white">
			<view class="font-30 color3 pt-5">需要隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，面板1.0，长度12米，牛腿，用过2个工地以上的不要，工地在遵义，贵州附近有货联系我</view>
			<view class="d-flex a-center">
				<image src="../../static/images/detailsl-icon.png" class="icon1"></image>
				<view class="font-24 color6 pl-1 py-3">252</view>
			</view>
			<view class="color6 font-24 d-flex j-sb a-start pb-3 borderB-f5">
				<view class="line-h">
					<view class="pb-2">首发时间：2018-2-23</view>
					<view>更新时间：2020-2-23</view>
					<!-- <view class="Rcolor">本条信息已失效</view> -->
				</view>
				<view class="d-flex a-center">
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon1.png" class="icon2"></image>
						<view class="pl-1">已收藏</view>
					</view>
					<view class="d-flex a-center ml-5" @click="changeShare()">
						<image src="../../static/images/detailsl-icon2.png" class="icon2"></image>
						<view class="pl-1">分享</view>
					</view>
				</view>
			</view>
			<view class="d-flex a-center j-sb h-100 pt-4">
				<view class="font-22 d-flex a-center">
					<view class="tag tagG">求购</view>
					<view><text class="tag tagR">介绍费</text><text class="tag tagO">1235</text></view>
					<!-- <view class="tag tagY">信息已失效</view> -->
				</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
					<view class="font-24 color6 pl-1">西安</view>
				</view>
			</view>
			<view class="font-22 Rcolor pt-2 pb-4">信息发布者愿意支付给帮忙成功介绍者介绍费（品台不提供担保）</view>
		</view>
		
		<!-- 个人详情信息 -->
		<view class="bg-white px-3 mt-2">
			<view class="py-4 d-flex a-center userBox">
				<image src="../../static/images/detailsl-img.png" class="userImg"></image>
				<view class="font-26 color2 ml-3 flex-1">
					<view class="pb-3">张瑾里</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
						<view>15623256894</view>
						<!-- <view>信息已失效，联系方式不可见</view> -->
					</view>
				</view>
			</view>
			<view class="line-h-md py-4 font-24 color6 dashedBorder">
				<view>我能承包：开挖班/初支班</view>
			</view>
		</view>
		
		<view class="bg-white mt-2 font-24 colorO text-center py-3">联系我时，请说是在劳务信息小程序上看到的信息哦！</view>
		
		<view class="py-5 font-26 Mcolor text-center" @click="Router.redirectTo({route:{path:'/pages/labour-publish/labour-publish'}})">我也要发布招工求职信息</view>
		
		
		<!-- 分享 -->
		<view class="bg-mask" v-show="share_show">
			<view class="bg-white rounded20-T color2 font-28 text-center position-absolute bottom-0 left-0 right-0">
				<view class="py-4">分享给</view>
				<view class="d-flex a-center j-sb share m-a mb-5 pt-3">
					<view class="font-26 pb-1"><image src="../../static/images/sharel-icon.png" class="img1"></image><view>微信好友</view></view>
					<view class="font-26 pb-1" @click="Router.redirectTo({route:{path:'/pages/labour-share/labour-share'}})"><image src="../../static/images/sharel-icon1.png" class="img2"></image><view>微信朋友圈</view></view>
				</view>
				<view class="font-26 py-3 bg-colorf5 mt-5 z-index100" @click="changeShare()">取消</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				share_show:false
			}
		},
		methods: {
			changeShare(){
				const self = this;
				self.share_show = !self.share_show;
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.banner{height: 505rpx;position: relative;}
.swiper-sign{background-color: #434343;position: absolute;bottom: 30rpx;right: 30rpx;}
.icon1{width: 31rpx;height: 23rpx;}
.icon2{width: 33rpx;height: 34rpx;}
.icon3{width: 36rpx;height: 34rpx;}
.icon4{width: 21rpx;height: 25rpx;}
.icon5{width: 20rpx;height: 26rpx;margin-right: 10rpx;}

.userImg{width: 100rpx;height: 100rpx;}

.tj .item image{width: 180rpx;height: 180rpx;}
.tj .itemCon .tit{width: 480rpx;line-height: 1.2;}
.tj .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.btn{width: 260rpx;line-height: 50rpx;}

.share{width: 440rpx;}
.share>view{display: flex;flex-direction: column;align-items: center;}
.img1{width: 83rpx;height: 70rpx;margin-bottom: 35rpx}
.img2{width: 76rpx;height: 77rpx;margin-bottom: 35rpx}

.tagR{margin-right: 0;}
.dashedBorder{border-top: 1px dashed #e1e1e1;}
</style>
