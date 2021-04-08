<template>
	<view>
		<view class="bg-white py-4 px-1 font-30 color2 text-center">一律禁止发布虚假、敏感、政治、煽动性、色、赌、毒等违法信息，一经发现，一律黑名单</view>
		
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white mt-2">
			<view class="font-30 color2">姓名</view>
			<input type="text" placeholder-class="font-30" class="color2 font-30" placeholder="三项信息至少填写一项" v-model="submitData.title"/>
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white">
			<view class="font-30 color2">身份证号码</view>
			<input type="idcard" placeholder-class="font-30" class="color2 font-30" placeholder="三项信息至少填写一项" v-model="submitData.description"/>
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-e1 px-3 bg-white">
			<view class="font-30 color2">手机号</view>
			<input type="number" placeholder-class="font-30" class="color2 font-30" maxlength="11" placeholder="三项信息至少填写一项" v-model="submitData.phone"/>
		</view>
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-30">不良行为说明({{submitData.content.length>100?100:submitData.content.length}}/100)</view>
			<textarea style="height: 210rpx;" placeholder-class="font-30" class="color2 font-30 w-100" v-model="submitData.content" maxlength="100" placeholder="请填写100个字以内的内容" />
		</view>
		<view class="d-flex a-center py-4 px-3 bg-white uploadImg" @click="upLoadImg('mainImg')">
			<view class="position-relative">
				<image :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:'../../static/images/the-queryl-icon2.png'" class="icon"></image>
				<image v-if="submitData.mainImg&&submitData.mainImg[0]" src="../../static/images/used-to-releasel-icon1.png" @click="deleteImg()" class="icon2"></image>
			</view>
			
			<view class="font-30 color2 pl-2">上传图片（选填）</view>
		</view>
		
		<view class="btn400" @click="Utils.stopMultiClick(submit)">提交</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Utils:this.$Utils,
				submitData:{
					title:'',
					phone:'',
					description:'',
					content:'',
					mainImg:[],
					type:4,
					thirdapp_id:2
				}
			}
		},
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
		},
		methods: {
			
			deleteImg(){
				const self = this;
				self.submitData.mainImg = [];
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.title==''&&self.submitData.phone==''&&self.submitData.description==''){
					self.$Utils.showToast('三项信息至少填写一项', 'none', 1000);
					uni.setStorageSync('canClick', true);
					return
				};
				if(self.submitData.content==''){
					self.$Utils.showToast('请填写不良信息说明', 'none', 1000);
					uni.setStorageSync('canClick', true);
					return
				};
				self.messageAdd();
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('登记成功', 'none', 1000)
						uni.setStorageSync('canClick', true);
						setTimeout(function() {
							self.submitData = {
								title:'',
								phone:'',
								description:'',
								content:'',
								mainImg:[],
								type:4,
								thirdapp_id:2
							}
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.icon{width: 80rpx;height: 80rpx;}
.btn400{margin-top: 100rpx;margin-bottom: 100rpx;}
</style>
