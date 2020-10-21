<template>
	<view>
		
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white borderB-f5">
			<view>主页顶部图片</view>
			<view class="d-flex a-center" @click="upLoadImg('mainImg')">
				<image :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:'../../static/images/bg.jpg'" class="icon1"></image>
				<image src="../../static/images/used-to-releasel-icon3.png" class="icon2"></image>
			</view>
		</view>
		<view class="font-30 color2 d-flex j-sb a-center py-4 px-3 bg-white ">
			<view>实名认证</view>
			<input placeholder-class="font-30" class="color2 font-30" type="text" v-if="submitData.company==''" placeholder="请填写" v-model="submitData.company"/>
			<view>{{submitData.company}}</view>
		</view>
		<!-- <view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h font-24">业务范围</view>
			<textarea value="" placeholder="请填写100个字以内的内容" class="border-e1 p-2" v-model="submitData.passage1"/>
		</view> -->
		<view style="color: red;text-align: right;line-height: 80rpx;" class="font-22 px-2">修改认证名称请联系客服：{{kefu}}</view>
		<view class="d-flex a-center j-sb py-4 borderB-f5 px-3 bg-white">
			<view class="font-28 color2">联系人</view>
			<input type="text" class="color2 font-30" placeholder="请填写" v-model="submitData.name"/>
		</view>
		<view class="d-flex a-center j-sb py-4 borderB-f5 px-3 bg-white">
			<view class="font-28 color2">联系电话</view>
			<input type="text" class="color2 font-30" placeholder="请填写" v-model="submitData.phone" />
		</view>
		<view class="py-4 borderB-f5 px-3 bg-white">
			<view class="d-flex a-center j-sb color2 pb-4 line-h">业务介绍</view>
			<textarea maxlength="40" class="color2 font-30 border-e1 p-2" placeholder="请填写40个字以内的内容"  v-model="submitData.passage2"/>
		</view>
		
		<view class="py-5"  @click="Utils.stopMultiClick(submit)">
			<view class="btn400">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				submitData:{
					name:'',
					company:'',
					phone:'',
					mainImg:[],
					//passage1:'',
					passage2:''
				},
				Utils:this.$Utils,
				kefu:''
			}
		},
		
		onLoad() {
			const self = this;
			self.kefu = uni.getStorageSync('kefu');
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
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
			
			
			submit() {
				const self = this;
				var newObject = self.$Utils.cloneForm(self.submitData);
				const pass = self.$Utils.checkComplete(newObject);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入真实有效的手机号', 'none', 1000)
						return;
					}
					self.userInfoUpdate();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('保存成功', 'none', 1000)
						uni.setStorageSync('canClick', true);
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
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
						self.submitData.name = self.userInfoData.name;
						self.submitData.phone = self.userInfoData.phone;
						self.submitData.company = self.userInfoData.company;
						self.submitData.mainImg = self.userInfoData.mainImg;
						//self.submitData.passage1 = self.userInfoData.passage1;
						self.submitData.passage2 = self.userInfoData.passage2;
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}

.icon1{width: 90rpx!important;height: 90rpx!important;margin-right: 20rpx!important;}
.icon2{width: 12rpx!important;height: 21rpx!important;}
textarea{width: 100%;box-sizing: border-box;}
</style>
