<template>
	<view>
		
		<view class="font-36 Rcolor pt-5 pb-4 text-center bg-white">恭喜您，发布成功！</view>
		<view class="bg-white">
			<view class="d-flex a-start font-24 color2 mb-3 mx-3">
				<view class="sgin"></view>
				<view>本条信息有效期至：2020.05.23（7天），可在[我的-发布的为首信息]里刷洗效果</view>
			</view>
			<view class="d-flex a-start font-24 color2 mb-3 mx-3">
				<view class="sgin"></view>
				<view>如果设备已出售/购得，请在[我的-发布二手信息]里取消交易，以免电话被骚扰</view>
			</view>
			<view class="d-flex mx-3 j-sa pt-2 pb-5">
				<view class="btn1" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">返回首页</view>
				<view class="btn1" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">查看发布信息</view>
			</view>
		</view>
		
		<view class="mt-2 px-3 bg-white problemBox">
			<view class="font-30 color2 pt-4 pb-1 line-h">常见问题</view>
			<view class="font-26 borderB-f5 py-3 problem">
				<view class="color2 pb-3">1、问题一问题一问题一问题一问题一问题一问题一问题， 一问题一问题一问题一</view>
				<view class="color6">答：回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答</view>
			</view>
			<view class="font-26 borderB-f5 py-3 problem">
				<view class="color2 pb-3">1、问题一问题一问题一问题一问题一问题一问题一问题， 一问题一问题一问题一</view>
				<view class="color6">答：回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答回答</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.sgin{width: 15rpx;height: 12rpx;background-color: #222;margin: 10rpx;margin-left: 0;transform: rotate(45deg);}
.btn1{background-color: #51A9E9;color: #fff;border-radius: 10rpx;text-align: center;line-height: 80rpx;width: 280rpx;}
</style>
