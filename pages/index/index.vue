<template>
	<view>
		<view class="head px-3 bg-mcolor z-index100" :style="{paddingTop:statusBar +'px'}">
			<view class="font-36 colorf d-flex a-center py-2">桥隧之家</view>
			<view class="colorf font-24 top overflow-h">欢迎来到桥隧之家，在这里您可以买卖二手设备，劳务招聘，在这里您可以买卖二手设备，劳务招聘</view>
		</view>
		
		<!-- banner -->
		<view class="banner">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
				<block v-for="(item,index) in 3" :key="index">
					<swiper-item class="swiper-item">
						<image src="../../static/images/home-banner.png" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<!-- 推荐 -->
		<view class="p-3 bg-white indexBox1">
			<view class="d-flex a-center j-sb">
				<view class="d-flex flex-column j-sb mr-3 h-100" @click="Router.redirectTo({route:{path:'/pages/newsDetail/newsDetail'}})">
					<view class="font-30 color2 flex-1 mb-3 avoidOverflow2 tjTit">隧道佳乐湿喷机，多年口碑，品质保证</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tag1">导购</view>
							<view class="tag tag1">湿喷机</view>
							<view class="tag tag2">推荐</view>
						</view>
						<view class="d-flex a-center">
							<image src="../../static/images/home-icon.png" class="icon1"></image>
							<view class="font-24 color6 pl-1">252</view>
						</view>
					</view>
				</view>
				<image src="../../static/images/home-img.png" class="tjImg"></image>
			</view>
			<view class="bg-f5 d-flex a-center p-1 mt-4">
				<image src="../../static/images/home-icon2.png" class="icon2"></image>
				<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
			</view>
		</view>
		
		<view class="mt-2 p-3 bg-white indexBox2" @click="Router.redirectTo({route:{path:'/pages/newsDetail/newsDetail'}})">
			<view class="font-30 color2">厂家直销各种胶皮手套、818止滑手套、线手套、电焊手套等各种施工手套</view>
			<view class="d-flex a-center mt-3 flex-wrap imgBox">
				<image src="../../static/images/home-img1.png" mode=""></image>
				<image src="../../static/images/home-img1.png" mode=""></image>
				<image src="../../static/images/home-img1.png" mode=""></image>
				<image src="../../static/images/home-img1.png" mode=""></image>
			</view>
			<view class="d-flex a-center j-sb h-100 mt-3">
				<view class="font-22 d-flex a-center">
					<view class="tag tag1">批发</view>
					<view class="tag tag1">劳保类</view>
				</view>
				<view class="d-flex a-center">
					<image src="../../static/images/home-icon.png" class="icon1"></image>
					<view class="font-24 color6 pl-1">252</view>
				</view>
			</view>
			<view class="bg-f5 d-flex a-center p-1 mt-4">
				<image src="../../static/images/home-icon2.png" class="icon2"></image>
				<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
			</view>
		</view>
		
		<view class="mt-2 p-3 bg-white indexBox3">
			<view class="font-30 color2">厂家直销各种胶皮手套、818止滑手套、线手套、电焊手套等各种施工手套</view>
			<view class="mt-3">
				<video class="video" controls poster="../../static/images/home-img2.png">
				    <source src="" type="video/mp4">
				    您的浏览器不支持 video 标签。
				</video>
			</view>
			<view class="d-flex a-center j-sb h-100 mt-3">
				<view class="font-24 color6">时长：12分钟</view>
				<view class="d-flex a-center">
					<image src="../../static/images/home-icon.png" class="icon1"></image>
					<view class="font-24 color6 pl-1">252</view>
				</view>
			</view>
			<view class="bg-f5 d-flex a-center p-1 mt-4">
				<image src="../../static/images/home-icon2.png" class="icon2"></image>
				<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
			</view>
		</view>
		
		
		<view class="py-5 font-26 color9 text-center">没有更多内容了</view>
		<view style="height: 100rpx;width: 100%;"></view>
		<!-- 底部 -->
		<view class="footer"> 
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
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
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar5.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				statusBar: app.globalData.statusBar,
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
		}
	};
</script>
<style>
page{background-color: #f5f5f5;}
.top{height: 65rpx;line-height: 65rpx;white-space: nowrap;}
.banner{height: 240rpx;}
.swiper-box{width: 100%;height: 100%;}
.banner image{width: 100%;height: 100%;}

.icon1{width: 30rpx!important;height: 22rpx!important;}
.tjTit{width: 480rpx;}
.tjImg{width: 210rpx;height: 160rpx;}
.icon2{width: 27rpx!important;height: 27rpx!important;}

.imgBox image{width: 210rpx;height: 160rpx;margin-right: 31rpx;}
.imgBox image:nth-child(3n){margin-right: 0;}
.imgBox image:nth-child(n+4){margin-top: 20rpx;}

.video{height: 340rpx;width: 100%;}

</style>
