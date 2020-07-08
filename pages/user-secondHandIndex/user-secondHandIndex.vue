<template>
	<view>
		
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white">
			<view>主页顶部图片</view>
			<view class="d-flex a-center">
				<image src="../../static/images/the queryl-icon2.png" class="icon1"></image>
				<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
			</view>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white mt-2 borderB-f5">
			<view>主页顶部图片</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon2"></image>
		</view>
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-24">业务范围</view>
			<textarea value="" placeholder="请填写800个字以内的内容" class="border-e1 p-2"/>
		</view>
		
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white mt-2">
			<view class="font-28 color2">联系人</view>
			<input type="text" placeholder="请填写" />
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white">
			<view class="font-28 color2">手机号</view>
			<input type="text" placeholder="请填写" />
		</view>
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-24">个性签名</view>
			<textarea value="" placeholder="请填写800个字以内的内容" class="border-e1 p-2"/>
		</view>
		
		<view class="py-5">
			<view class="btn400">保存</view>
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
.icon1{width: 90rpx;height: 90rpx;margin-right: 20rpx;}
.icon2{width: 12rpx;height: 21rpx;}
textarea{width: 100%;box-sizing: border-box;}
</style>
