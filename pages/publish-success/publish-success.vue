<template>
	<view>
		
		<view class="font-36 Rcolor pt-5 pb-4 text-center bg-white">恭喜您，发布成功！</view>
		<view class="bg-white">
			<view class="d-flex a-start font-24 color2 mb-3 mx-3">
				<view class="sgin"></view>
				<view>本条信息有效期至：{{mainData.invalid_time?mainData.invalid_time:''}}（7天），可在[我的-发布的二手信息]里刷新有效期</view>
			</view>
			<!-- <view class="d-flex a-start font-24 color2 mb-3 mx-3">
				<view class="sgin"></view>
				<view>如果设备已出售/购得，请在[我的-发布二手信息]里取消交易，以免电话被骚扰</view>
			</view> -->
			<view class="d-flex mx-3 j-sa pt-2 pb-5">
				<view class="btn1" @click="Router.redirectTo({route:{path:'/pages/secondHand/secondHand'}})">返回二手列表</view>
				<view class="btn1" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+id}})">查看发布信息</view>
			</view>
		</view>
		
		<view class="mt-2 px-3 pb-3 bg-white problemBox">
			<view class="font-30 color2 pt-4 pb-1 line-h">常见问题</view>
			<view class="content ql-editor" style="padding:0;" v-html="artData.content">
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				id:'',
				artData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getArtData'], self);
		},
		
		
		methods: {
			
			getArtData() {
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
							title: ['in', ['常见问题']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.artData = res.info.data[0];
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getArtData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.invalid_time = self.$Utils.timeto(self.mainData.invalid_time*1000,'ymd')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.sgin{width: 15rpx;height: 12rpx;background-color: #222;margin: 10rpx;margin-left: 0;transform: rotate(45deg);}
.btn1{background-color: #51A9E9;color: #fff;border-radius: 10rpx;text-align: center;line-height: 80rpx;width: 280rpx;}
</style>
