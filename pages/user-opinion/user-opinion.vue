<template>
	<view>
		
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-32">提意见或建议</view>
			<textarea value="" placeholder="请填写800个字以内的内容" class="border-e1 p-2 font-30"/>
		</view>
		<view class="font-30 color2 py-4 px-3 bg-white">
			<view>添加图片</view>
			<!-- 添加图片 -->
			<view class="uploadImg d-flex a-center">
				<view class="position-relative">
					<image src="../../static/images/laborl-img.png" mode=""></image>
					<image src="../../static/images/used to releasel-icon1.png" class="icon2"></image>
				</view>
				<image src="../../static/images/used to releasel-icon.png" mode=""></image>
			</view>
			
		</view>
		
		<button class="btn400" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					description:'',
					type:5
				}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('留言成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			


			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					const callback = (user, res) => {
						console.log(res)
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);
				
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入留言内容', 'none')
				};
			},
		}
	};
</script>


<style>
page{background-color: #f5f5f5;}
textarea{width: 100%;box-sizing: border-box;height: 600rpx;}
.icon1{width: 12rpx;height: 21rpx;}
.btn400{margin: 130rpx auto 80rpx;}
</style>
