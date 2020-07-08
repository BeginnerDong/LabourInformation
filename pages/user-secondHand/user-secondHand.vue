<template>
	<view>
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">求购</view>
		</view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-e1">
			<view>选择</view>
			<view class="Mcolor d-flex a-center">
				<view class="pr-5 borderR-f5" @click="showToast()">删除</view>
				<view class="pl-5">一键更新日期</view>
			</view>
		</view>
		
		<view>
			<view class="borderB-e1 py-4">
				<view class="d-flex a-center mx-3">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="ml-2 d-flex a-center j-sb flex-1">
						<image src="../../static/images/second-handl-img1.png" class="img"></image>
						<view class="itemCon flex-1 ml-2">
							<view class="color3 font-28 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
							<view class="font-22 color6 d-flex a-center j-sb mt-2 line-h">
								<view class="Rcolor">面议</view>
								<view>贵阳</view>
							</view>
							<view class="font-22 d-flex a-center h-100 mt-2">
								<view class="tag tagB">出售</view>
								<view class="tag tagO">湿喷机</view>
							</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 py-3 mx-3 d-flex j-end">
					<view>首发时间：2020.05.23</view>
					<view class="pl-2">最近更新时间：2020.06.01</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="showToast()">删除</view>
					<view class="borderL-e1 borderR-e1">编辑</view>
					<view>更新时间</view>
				</view>
			</view>
			
			<view class="borderB-e1 py-4">
				<view class="d-flex a-center mx-3">
					<!-- <image src="../../static/images/i releasel-icon.png" class="icon1"></image> -->
					<image src="../../static/images/i releasel-icon1.png" class="icon1"></image>
					<view class="ml-2 d-flex a-center j-sb flex-1">
						<image src="../../static/images/second-handl-img1.png" class="img"></image>
						<view class="itemCon flex-1 ml-2">
							<view class="color3 font-28 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
							<view class="font-22 color6 d-flex a-center j-sb mt-2 line-h">
								<view class="Rcolor">面议</view>
								<view>贵阳</view>
							</view>
							<view class="font-22 d-flex a-center h-100 mt-2">
								<view class="tag tagG">出售</view>
								<view class="tag tagO">湿喷机</view>
							</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 py-3 mx-3 d-flex j-end">
					<view>首发时间：2020.05.23</view>
					<view class="pl-2">最近更新时间：2020.06.01</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="showToast()">删除</view>
					<view class="borderL-e1 borderR-e1">编辑</view>
					<view>更新时间</view>
				</view>
			</view>
			
		</view>
		
		
		<!-- 删除弹框 -->
		
		
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
				self.navCurr = i
			},
			showToast(){
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定删除该条信息？'
				})
			}
		}
	}
</script>

<style>
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 40rpx;height: 40rpx;}
.img{width: 160rpx;height: 160rpx;}

.oh view{width: 33.33%;}
</style>
