<template>
	<view>
		
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-32">
				
				<view>您的意见或建议、相关投诉</view>
				<view class="font-24">({{submitData.description.length>200?200:submitData.description.length}}/200)</view>
			</view>
			<textarea maxlength="200" v-model="submitData.description" placeholder="请填写200个字以内的内容" class="border-e1 p-2 font-30"/>
		</view>
		<view class="font-30 color2 py-4 px-3 bg-white">
			<view>添加图片（选填）</view>
			<!-- 添加图片 -->
			<view class="uploadImg d-flex a-center">
				<view class="position-relative" v-for="(item,index) of submitData.mainImg" :key="index">
					<image :src="item.url" mode=""></image>
					<image src="../../static/images/used-to-releasel-icon1.png" @click="deleteImg(index)" class="icon2"></image>
				</view>
				<image v-if="submitData.mainImg.length<3" @click="upLoadImg('mainImg')" src="../../static/images/used-to-releasel-icon.png"
				 mode=""></image>
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
					type:5,
					mainImg:[]
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 3-self.submitData.mainImg.length,
					success: function(res) {
						var tempFilePaths = res.tempFilePaths;
						for (var i = 0; i < tempFilePaths.length; i++) {
							var file = res.tempFiles[i];
							var obj = res.tempFiles[i].path.lastIndexOf(".");
							var ext = res.tempFiles[i].path.substr(obj+1);
							console.log(callback)
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
							}, callback)
						}
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			deleteImg(e){
				const self = this;
				self.submitData.mainImg.splice(parseInt(e),1)
			},
			
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
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.mainImg;
				const pass = self.$Utils.checkComplete(newObject);
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
textarea{width: 100%;box-sizing: border-box;height: 465rpx;}
.icon1{width: 12rpx;height: 21rpx;}
.btn400{margin: 130rpx auto 80rpx;}
</style>
