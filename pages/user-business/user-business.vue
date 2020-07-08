<template>
	<view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-e1">
			<view>选择</view>
			<view class="Mcolor d-flex a-center">
				<view class="pr-5 borderR-f5" @click="showToast()">删除</view>
				<view class="pl-5">一键更新日期</view>
			</view>
		</view>
		
		<view>
			
			<view class="rounded shadow m-2 pb-3">
				<view class="px-2 d-flex a-center">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="flex-1 ml-2">
						<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
						<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
							<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
							<view class="d-flex a-center">
								<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
								<view class="font-24 color6 pl-1">西安</view>
							</view>
						</view>
						<view class="d-flex font-24 pt-3">
							<view class="color6">业务范围：</view>
							<view class="color2 flex-1">
								<view>隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
							</view>
						</view>
						<view class="d-flex font-24 py-3">
							<view class="color6">销售区域：</view>
							<view class="color2 flex-1">全国</view>
						</view>
					</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="showToast()">删除</view>
					<view class="borderL-e1 borderR-e1">编辑</view>
					<view>更新时间</view>
				</view>
			</view>
			
			<view class="rounded shadow m-2 pb-3">
				<view class="px-2 d-flex a-center">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="flex-1 ml-2">
						<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
						<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
							<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
							<view class="d-flex a-center">
								<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
								<view class="font-24 color6 pl-1">西安</view>
							</view>
						</view>
						<view class="d-flex font-24 pt-3">
							<view class="color6">业务范围：</view>
							<view class="color2 flex-1">
								<view>隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
								<view class="d-flex a-center flex-wrap">
									<image src="../../static/images/second-handl-img1.png" class="img"></image>
									<image src="../../static/images/second-handl-img1.png" class="img"></image>
									<image src="../../static/images/second-handl-img1.png" class="img"></image>
								</view>
							</view>
						</view>
						<view class="d-flex font-24 py-3">
							<view class="color6">销售区域：</view>
							<view class="color2 flex-1">全国</view>
						</view>
					</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="showToast()">删除</view>
					<view class="borderL-e1 borderR-e1">编辑</view>
					<view>更新时间</view>
				</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
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
.icon1{width: 40rpx;height: 40rpx;}
.icon3{width: 21rpx;height: 25rpx;}
.img{width: 106rpx;height: 106rpx;margin-top: 20rpx;margin-right: 20rpx;}
.img:nth-child(4n){margin-right: 0;}
.shadow{box-shadow: 0 0 16rpx rgba(114, 130, 138, 0.2)!important;}
.oh view{width: 33.33%;}
</style>
