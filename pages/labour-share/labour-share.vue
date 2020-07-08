<template>
	<view>
		
		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-3">分享到朋友圈步骤</view>
			<view><text>1</text>先保存图片><text>2</text>将图片发送到朋友圈</view>
			<view>他人可通过点开图片长安识别小程序码，查看本条信息</view>
		</view>
		
		
		<view class="bg-white mt-2 px-3">
			<view class="py-3 font-32 color2 text-center">保存图片样式</view>
			<view class="rounded10 overflow-h shadow pb-3">
				<!-- 招聘分享 -->
				<!-- <view>
					<view class="font-30 color2 d-flex a-center j-sb pr-2 pb-2">
						<view><text class="sgin">贵阳工地</text><text class="pl-2">需要</text></view>
						<view>发布日期：2020-06-25</view>
					</view>
					<view class="font-32 color2 px-2 line-h-md">
						<view><text class="Mcolor">电焊工，</text>5名，月工资6000</view>
						<view><text class="Mcolor">装载机司机，</text>5名，月工资6000</view>
						<view><text class="Mcolor">挖掘机司机，</text>5名，月工资6000</view>
					</view>
					<view class="d-flex a-center j-sb mx-2 line-h py-3 borderB-f5 font-24 color2">
						<view>更多>></view>
						<view class="Rcolor">介绍成功给100介绍费</view>
					</view>
				</view> -->
				<!-- 找活分享 -->
				<view>
					<view class="font-30 color2 d-flex a-center j-sb pr-2 pb-2">
						<view><text class="sgin">工人找活</text></view>
						<view>发布日期：2020-06-25</view>
					</view>
					<view class="font-32 color2 mx-2 line-h-md borderB-f5 pb-3">
						<view class="font-36 Mcolor font-w text-center pb-2">装载机司机</view>
						<view>本人目前待业在家，10年隧道装载机经验，出渣速度快，服务管理安排，吃苦耐劳，有需要开装载机司机的老板联系我</view>
					</view>
				</view>
				
				<view class="font-24 color2 text-center py-3">长安图片，识别图中小程序码，<text class="font-w">免费查看联系方式</text></view>
				<view class="shareImg d-flex j-sb a-center px-3">
					<image src="../../static/images/used to sharel-img.png" mode=""></image>
					<image src="../../static/images/used to sharel-img1.png" mode=""></image>
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
.shareImg image{width: 260rpx;height: 260rpx;}
.shareImg image:nth-child(2){width: 288rpx;height: 258rpx;}

.sgin{background-color: #D30D11;color: #fff;border-top-left-radius: 10rpx;border-bottom-right-radius: 10rpx;width: 160rpx;line-height: 60rpx;text-align: center;display: inline-block;}
</style>
