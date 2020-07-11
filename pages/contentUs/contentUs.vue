<template>
	<view>
		
		<view class="px-3">
			<view class="py-4 borderB-e1 d-flex a-center">
				<image src="../../static/images/contact usl-icon.png" class="icon"></image>
				<view class="font-28 color2 pl-1">{{mainData.phone}}</view>
			</view>
			<view class="py-4 d-flex a-center">
				<image src="../../static/images/contact usl-icon1.png" class="icon"></image>
				<view class="font-28 color2 pl-1">{{mainData.description}}</view>
			</view>
		</view>
		<view style="background: #f5f5f5;height: 20rpx;"></view>
		<view class="px-3 mt-2">
			<view class="font-30 color2 pt-4">公司简介</view>
			<view class="font-26 color6 py-3 line-h-md">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	
	export default {
		
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['关于我们']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
.icon{width: 27rpx;height: 33rpx;}
.icon:nth-child(2){width: 28rpx;height: 33rpx;}
.img{width: 100%;height: 400rpx;}
</style>
