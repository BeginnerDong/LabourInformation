<template>
	<view>
		
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-32">提意见或建议</view>
			<textarea value="" placeholder="请填写800个字以内的内容" class="border-e1 p-2 font-30"/>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white">
			<view>联系我们</view>
			<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
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
textarea{width: 100%;box-sizing: border-box;height: 600rpx;}
.icon1{width: 12rpx;height: 21rpx;}
.btn400{margin-top: 130rpx;}
</style>
