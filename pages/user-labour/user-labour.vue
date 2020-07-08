<template>
	<view>
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">招工人</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">招队伍</view>
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">工人找活</view>
			<view class="item" :class="navCurr==5?'on':''" @click="changeNav(5)">队伍找活</view>
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
				<view class="d-flex a-center ml-3">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="bg-white px-3 flex-1" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
						<view class="font-30 color2 pt-4">隧道施工队继续开挖班，处置办，洞1千米长，三级围岩，有经验的队伍请来咨询</view>
						<view class="d-flex j-sb flex-wrap pt-3 imgBox">
							<image src="../../static/images/laborl-img.png" mode=""></image>
							<image src="../../static/images/laborl-img.png" mode=""></image>
							<image src="../../static/images/laborl-img.png" mode=""></image>
							<image src="../../static/images/laborl-img.png" mode=""></image>
						</view>
						<view class="d-flex a-center j-sb py-3">
							<view>
								<view class="tag tagB">招队伍</view>
								<view class="tag tagY">信息已失效</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/detailsl-icon3.png" class="icon2"></image>
								<view class="font-24 color6 pl-1">西安</view>
							</view>
						</view>
						<view class="line-h-md font-24 color6 dashedBorder">
							<view>开挖班，1支队伍，待遇面议</view>
							<view>初支班，1支队伍，待遇面议</view>
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
				<view class="d-flex a-center ml-3">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="bg-white px-3 flex-1" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
						<view class="font-30 color2 pt-4">隧道施工队继续开挖班，处置办，洞1千米长，三级围岩，有经验的队伍请来咨询</view>
						<view class="d-flex a-center j-sb py-3">
							<view>
								<view class="tag tagB">招队伍</view>
								<view class="tag tagR">介绍费</view>
								<view class="tag tagO">1235</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/detailsl-icon3.png" class="icon2"></image>
								<view class="font-24 color6 pl-1">西安</view>
							</view>
						</view>
						<view class="line-h-md font-24 color6 dashedBorder">
							<view>开挖班，1支队伍，待遇面议</view>
							<view>初支班，1支队伍，待遇面议</view>
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
.nav .item{padding: 0 30rpx;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 40rpx;height: 40rpx;}

.oh view{width: 33.33%;}

.imgBox image{width: 145rpx;height: 145rpx;}
.icon2{width: 21rpx;height: 25rpx;}
.tagR{margin-right: 0;}
</style>
