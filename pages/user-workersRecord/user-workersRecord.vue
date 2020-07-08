<template>
	<view>
		<view class="bg-white py-4 font-30 color2 text-center">工人不良行为登记</view>
		
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white mt-2">
			<view class="font-28 color2">联系人</view>
			<input type="text" placeholder="必填" />
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white">
			<view class="font-28 color2">省份证号码</view>
			<input type="text" placeholder="选填" />
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white">
			<view class="font-28 color2">手机号</view>
			<input type="text" placeholder="选填" />
		</view>
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-24">不良行为说明（0/100）</view>
			<textarea value="" placeholder="请填写100个字以内的内容" class="w-100"/>
		</view>
		<view class="d-flex a-center py-4 px-3 bg-white">
			<image src="../../static/images/the queryl-icon2.png" class="icon"></image>
			<view class="font-28 color2 pl-2">上传图片（选填）</view>
		</view>
		
		<view class="btn400">提交</view>
		
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
.icon{width: 80rpx;height: 80rpx;}
.btn400{margin-top: 100rpx;margin-bottom: 100rpx;}
</style>
