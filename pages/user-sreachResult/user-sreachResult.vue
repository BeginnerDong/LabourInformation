<template>
	<view>
		
		<view class="font-24 color2 p-3 head position-relative">
			<text class="Rcolor">特别声明：</text>
			本次查询的内容均为网友提交，不能代表信息的真实性，仅供参考，如有涉及不实信息请及时与我们联系，我们将第一时间进行删除
			<text class="Mcolor" style="float: right;">举报不实信息</text>
		</view>
		<view style="height: 20rpx;" class="bg-f5"></view>
		<view class="font-28 color2 py-3 text-center">
			本次共查询100条信息，其中<text class="Rcolor">2</text>条涉及不良行为
		</view>
		<view style="height: 20rpx;" class="bg-f5"></view>
		
		<!-- 结果 -->
		<view>
			<view class="font-26 text-center color9 pt-5 mt-3">未查询到不良结果</view>
			
			<view class="font-26 color2">
				<view class="borderB-f5 p-3">
					<view class="pb-3">姓名：张三丰</view>
					<view class="pb-3">手机号：156****1258</view>
					<view class="pb-3">身份证号：无</view>
					<view class="d-flex">
						<view>不良记录：</view>
						<view class="flex-1 line-h-md">个位老总，首都机场不良记录不良记录不良记录不良记录不良记录不良记录不良记录不良记录</view>
					</view>
				</view>
				<view class="borderB-f5 p-3">
					<view class="pb-3">姓名：张三丰</view>
					<view class="pb-3">手机号：156****1258</view>
					<view class="pb-3">身份证号：无</view>
					<view class="d-flex">
						<view>不良记录：</view>
						<view class="flex-1 line-h-md">个位老总，首都机场</view>
					</view>
					<view class="d-flex">
						<view class="pt-2">相关图片：</view>
						<view class="flex-1 d-flex flex-wrap">
							<image src="../../static/images/laborl-img.png" class="img"></image>
							<image src="../../static/images/laborl-img.png" class="img"></image>
							<image src="../../static/images/laborl-img.png" class="img"></image>
							<image src="../../static/images/laborl-img.png" class="img"></image>
							<image src="../../static/images/laborl-img.png" class="img"></image>
						</view>
					</view>
				</view>
			</view>
			
			<view style="height: 100rpx;" class="bg-f5"></view>
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
.head{background-color: #FFF4D4;}
.img{width: 140rpx;height: 140rpx;margin-right: 20rpx;margin-top: 20rpx;}
</style>
