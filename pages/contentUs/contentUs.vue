<template>
	<view>
		<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
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
					thirdapp_id:2,
					title:['in','关于我们']
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
