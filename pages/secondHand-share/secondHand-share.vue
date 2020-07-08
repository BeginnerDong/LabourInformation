<template>
	<view>
		
		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-3">分享到朋友圈步骤</view>
			<view><text>1</text>先保存图片><text>2</text>将图片发送到朋友圈</view>
			<view>他人可通过点开图片长安识别小程序码，查看本条信息</view>
		</view>
		
		<!-- 公司分享 -->
		<!-- <view class="bg-white mt-2 px-3">
			<view class="py-3 font-28 color2 d-flex a-center j-sa">
				<view class="d-flex a-center"><image src="../../static/images/used to sharel-icon.png" class="icon1"></image>图片样式1</view>
				<view class="d-flex a-center"><image src="../../static/images/used to sharel-icon1.png" class="icon1"></image>图片样式2</view>
			</view>
			<view class="rounded10 overflow-h shadow pb-3">
				<view class="colorf bg-colorR py-4">
					<view class="font-50 text-center pb-3">更多二手供需信息</view>
					<view class="d-flex a-center j-center">
						<view class="font-28 d-flex a-center"><image src="../../static/images/used to sharel-icon2.png" class="icon2"></image>张瑾里</view>
						<view class="font-28 d-flex a-center ml-5"><image src="../../static/images/used to sharel-icon3.png" class="icon3"></image>15689745823</view>
					</view>
				</view>
				<view class="shareImg d-flex j-sb a-center px-3 pt-4">
					<image src="../../static/images/used to sharel-img.png" mode=""></image>
					<image src="../../static/images/used to sharel-img1.png" mode=""></image>
				</view>
				<view class="Rcolor font-36 text-center py-3">长按图片，识别图中小程序</view>
				<view class="font-24 color6 text-right d-flex a-center j-sa">
					<view class="borderR-e1 px-1">如果长安图片没有识别出小程序信息，<br />请重新点开图片，在长安识别，多试几次就行了</view>
					<image src="../../static/images/used%20to%20sharel-img2.png" class="img"></image>
				</view>
			</view>
				
			<view class="py-5">
				<view class="btn400">保存图片</view>
			</view>
		</view> -->
		
		
		<!-- 出售求购分享 -->
		<view class="bg-white mt-2 px-3">
			<view class="py-3 font-32 color2 text-center">图片样式</view>
			<view class="rounded10 overflow-h shadow pb-3">
				<view class="d-flex flex-wrap pr-1">
					<image src="../../static/images/second-handl-img2.png" class="img1"></image>
					<view class="p-2 color6 w-50"><text class="Rcolor font-w">出售</text>正同半径5.23米的价款台车一部，面板1.0，长度6米，只用一个工地，价格非常美丽需要的抓紧联系</view>
					<view class="w-50 pt-3">
						<image src="../../static/images/used to sharel-img.png"  class="img2"></image>
						<view class="font-22 color6 px-5 text-center">长安识别图中小程序码查看详情</view>
					</view>
					<view class="w-50 pt-2">
						<view class="font-32 Mcolor d-flex a-center pb-1"><image src="../../static/images/second-handl-icon1.png" class="icon4"></image>贵阳</view>
						<view class="font-28 color2 d-flex line-h a-center j-sb borderT-e1 py-2">
							<view class="d-flex a-center"><image src="../../static/images/second-handl-icon2.png"  class="icon5"></image>张经理</view>
							<view class="d-flex a-center"><image src="../../static/images/second-handl-icon3.png"  class="icon6"></image>15632864589</view>
						</view>
						<view class="font-22 color6 pt-2 border-e1 txt pl-1 line-h">
							<view class="pb-2">桥隧之家-完全免费的信息平台</view>
							<view class="pb-2">您可以</view>
							<view class="pb-2">1.快速的出售/求购二手机械设备</view>
							<view class="pb-2">2.快速查询劳务相关资源供应商</view>
							<view class="pb-2">3.快速招民工/班组，或者求职</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 text-center pt-3">没有识别出小程序信息，请重新点开图片，多试几次就行了</view>
			</view>
				
			<view class="py-5">
				<view class="btn400">保存图片</view>
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
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.head text{border: 1px solid #D30D11;color: #D30D11;border-radius: 50%;width: 24rpx;display: inline-block;line-height: 1;}

.icon1{width: 28rpx;height: 28rpx;margin-right: 20rpx;}
.icon2{width: 30rpx;height: 32rpx;margin-right: 10rpx;}
.icon3{width: 22rpx;height: 30rpx;margin-right: 10rpx;}
.icon4{width: 23rpx;height: 27rpx;margin-right: 10rpx;}
.icon5{width: 22rpx;height: 28rpx;margin-right: 10rpx;}
.icon6{width: 20rpx;height: 28rpx;margin-right: 10rpx;}
.shareImg image{width: 296rpx;height: 296rpx;}
.shareImg image:nth-child(2){width: 323rpx;height: 290rpx;}

.img{width: 150rpx;height: 62rpx;margin-left: 10rpx;}

.txt{width: 330rpx ;}
.img1{width:330rpx;height: 330rpx;}
.img2{width:230rpx;height: 230rpx;margin: 0 auto;}
</style>
