<template>
	<view>
		<view class="head position-relative">
			<image src="../../static/images/home-banner.png " mode=""></image>
			<view class="txt px-3 font-32 colorf">贵阳**机械设备有限公司 <view class="tag tagName1 ml-2">已实名认证</view></view>
		</view>
		
		<view class="px-3 bg-white">
			<view class="font-26 color9 py-3 borderB-e1 position-relative">我公司主营隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的<image src="../../static/images/labor%20releasel-icon1.png" class="icon1"></image></view>
			<view class="bg-white py-4 d-flex a-start">
				<view>
					<image src="../../static/images/detailsl-img.png" class="userImg"></image>
					<view class="Rcolor text-center font-18 pt-1">热度98561</view>
				</view>
				<view class="ml-3 flex-1">
					<view class="font-26 color2 pb-4 flex-1 d-flex a-center">
						<view class="pr-4">张瑾里</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon4.png" class="icon2"></image>
							<view>15623256894</view>
						</view>
					</view>
					<view class="font-24 color6">让客户买到物美价廉的二手设备是我们的初心，期待与各位新老顾客的长期合作</view>
				</view>
			</view>
			<view class="Mcolor font-24 text-center d-flex a-center j-sa pb-4 ">
				<view class="Mborder rounded10 btn">我要收藏</view>
				<view class="Mborder rounded10 btn">我要分享</view>
				<view class="Mborder rounded10 btn">拨打电话</view>
			</view>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 shadow-sm mt-2 bg-white nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">求购</view>
		</view>
		
		<!-- 列表 -->
		<view class="list">
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<image src="../../static/images/second-handl-img1.png" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>贵阳**有限公司 <view class="tag tagName">已实名认证</view></view>
						<view>贵阳</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagB">出售</view>
							<view class="tag tagO">湿喷机</view>
							<view class="tag tagR">置顶</view>
						</view>
						<view class="font-22 color9">1小时前</view>
					</view>
				</view>
			</view>
			
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<view class="font-50 colorf text-center itemConL">求购</view>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">需要一台220挖机，有合适的记得联系我</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>林先生<view class="tag tagName">已实名认证</view></view>
						<view>遵义</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagG">求购</view>
							<view class="tag tagO">湿喷机</view>
						</view>
						<view class="font-22 color9">昨天</view>
					</view>
				</view>
			</view>
			
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<image src="../../static/images/second-handl-img1.png" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>林先生<view class="tag tagName">已实名认证</view></view>
						<view>昆明</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagB">出售</view>
							<view class="tag tagO">湿喷机</view>
							<view class="tag tagY">信息已失效</view>
						</view>
						<view class="font-22 color9">7-23</view>
					</view>
				</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:1
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i;
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.head image{height: 240rpx;}
.head .txt{line-height: 80rpx;position: absolute;bottom: 0;left: 0;right: 0;background-color: rgba(0,0,0,0.5);}

.icon1{width: 19rpx;height: 10rpx;position: absolute;bottom: -10rpx;right: 0;}

.icon2{width: 20rpx;height: 26rpx;margin-right: 10rpx;}
.userImg{width: 100rpx;height: 100rpx;}
.btn{width: 200rpx;line-height: 50rpx;}

.nav image{width: 13rpx;height: 6rpx;margin-left: 8rpx;}
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.list .item image{width: 180rpx;height: 180rpx;}
.list .itemCon .tit{width: 480rpx;line-height: 1.2;}
.list .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}
</style>
