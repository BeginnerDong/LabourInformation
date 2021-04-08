<template>
	<view>
		<view class="bg-white px-3 pb-4">
			<view class="py-4 d-flex a-center j-sb">
				<view class="font-24 color6 d-flex a-center" @click="set">
					<image src="../../static/images/the-queryl-icon.png" class="icon1"></image>
					<view class="pl-2">粘贴</view>
				</view>
				<view class="font-24 color6 d-flex a-center" @click="deleteText">
					<image src="../../static/images/the-queryl-icon1.png" class="icon1"></image>
					<view class="pl-2">清空</view>
				</view>
			</view>
			
			<view class="position-relative">
				<textarea value="" placeholder=" " v-model="text"/>
				<view class="position-absoluteXY color9 font-24 p-3 line-h-lg" style="top: -4px;" v-if="text==''">
					<view>请输入要查询的名字/身份证号或手机号</view>
					<view>一行一条信息，否则查询不准</view>
					<view>可批量查询，长按可粘贴</view>
					<view class="pt-5">格式：</view>
					<view>张三</view>
					<view>李四</view>
					<view>13788889999</view>
					<view>401234199901011234</view>
				</view>
			</view>
			
		</view>
			
		<view class="btn400" @click="Utils.stopMultiClick(submit)">查询</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				text:'',
				infoArray:[],
				Utils:this.$Utils,
				Router:this.$Router
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick', true);
		},
		
		methods: {
			
			set(){
				const self = this;
				uni.getClipboardData({
					success(res) {
						if(res.data!=''){
							self.text = res.data
						}else{
							self.$Utils.showToast('剪贴板无内容', 'none', 1000);
						}
					}
				})
			},
			
			deleteText(){
				const self = this;
				self.text = '';
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.infoArray = self.text.split(/[(\r\n)\r\n]+/);
				console.log('self.infoArray',self.infoArray);
				for (var i = 0; i < self.infoArray.length; i++) {
					self.infoArray[i] = self.infoArray[i].replace(/\s*/g,"") 
					self.infoArray[i] = self.infoArray[i].toLocaleLowerCase()
				};
				if(self.infoArray.length==0||(self.infoArray[0]==''&&self.infoArray.length==0)){
					self.$Utils.showToast('信息填写不合规', 'none', 1000);
					uni.setStorageSync('canClick', true);
					return
				};
				if(self.infoArray[0]==''){
					self.infoArray.splice(0,1)
				}
				self.searchBad();
			},
			
			searchBad() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.keywords =self.infoArray;
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						uni.setStorageSync('canClick', true);
						uni.setStorageSync('infoNum',self.infoArray.length);
						uni.setStorageSync('infoData',data.info.data);
						self.Router.navigateTo({route:{path:'/pages/user-sreachResult/user-sreachResult'}})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.searchBad(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.icon1{width: 27rpx!important;height: 28rpx!important;}
textarea{width: 100%;height: 700rpx;box-shadow: 0 0 16rpx rgba(114, 130, 138, 0.2);box-sizing: border-box;padding: 30rpx;z-index: 99;}
.btn400{margin-top: 70rpx;margin-bottom: 70rpx;}
</style>
