<template>
	<view>
		<view class="bg-white py-4 font-30 color2 text-center mb-2">工人不良行为登记</view>
		
		<view class="bg-white px-3 pb-4">
			<view class="py-4 d-flex a-center j-sb">
				<view class="font-24 color6 d-flex a-center">
					<image src="../../static/images/the queryl-icon.png" class="icon1"></image>
					<view class="pl-2">粘贴</view>
				</view>
				<view class="font-24 color6 d-flex a-center">
					<image src="../../static/images/the queryl-icon1.png" class="icon1"></image>
					<view class="pl-2">清空</view>
				</view>
			</view>
			
			<view class="position-relative">
				<textarea value="" placeholder=" " />
				<view class="position-absoluteXY color9 font-24 p-3 line-h-lg">
					<view>请输入要查询的名字/省份证号或手机号</view>
					<view>一行一条信息，否则查询不准</view>
					<view>可批量查询，长按可粘贴</view>
					<view class="pt-5">实例格式：</view>
					<view>张三</view>
					<view>李四</view>
					<view>156829847586</view>
					<view>40158248793951258</view>
				</view>
			</view>
			
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
.icon1{width: 27rpx;height: 28rpx;}
textarea{width: 100%;height: 700rpx;box-shadow: 0 0 16rpx rgba(114, 130, 138, 0.2);box-sizing: border-box;padding: 30rpx;z-index: 99;}
.btn400{margin-top: 70rpx;margin-bottom: 70rpx;}
</style>
