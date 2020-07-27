<template>
	<view>
		<!-- banner -->
		<view class="banner">
			<swiper class="swiper-box h-100" autoplay="autoplay" interval="4000" :current="currIndex">
				<block v-for="(item,index) in mainData.mainImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" @click="preview(index)"/>
					</swiper-item>
				</block>
			</swiper>
			<view class="swiper-sign colorf font-20 line-h-md px-1 rounded10">{{currIndex+1}}/{{mainData.mainImg?mainData.mainImg.length:0}}</view>
		</view>
		
		<view class="px-3 bg-white">
			<view class="font-30 color3 pt-5">{{mainData.title}}</view>
			<view class="d-flex a-center">
				<image src="../../static/images/detailsl-icon.png" class="icon1"></image>
				<view class="font-24 color6 pl-1 py-3">{{mainData.view_count}}</view>
			</view>
			<view class="color6 font-24 d-flex j-sb a-start pb-3 borderB-f5">
				<view class="line-h">
					<view class="pb-2">首发时间：{{mainData.create_time?mainData.create_time:''}}</view>
					<view v-if="now < mainData.invalid_time">更新时间：{{mainData.update_time?mainData.update_time:''}}</view>
					<view class="Rcolor" v-else>本条信息已失效</view>
				</view>
				<view class="d-flex a-center">
					<view class="d-flex a-center"  @click="Utils.stopMultiClick(collect)">
						<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/detailsl-icon1.png':'../../static/images/detailsl-icon5.png'" class="icon2"></image>
						<view class="pl-1">{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'我要收藏'}}</view>
					</view>
					<view class="d-flex a-center ml-5" @click="changeShare()">
						<image src="../../static/images/detailsl-icon2.png" class="icon2"></image>
						<view class="pl-1">分享</view>
					</view>
				</view>
			</view>
			<view class="d-flex a-center j-sb h-100 py-4">
				<view class="font-22 d-flex a-center">
					<view class="tag tagB" v-if="mainData.behavior==1">出售</view>
					<view class="tag tagG" v-if="mainData.behavior==2">求购</view>
					<view class="tag tagO">{{mainData.label?mainData.label.title:''}}</view>
					<view class="tag tagP">{{mainData.behavior?(mainData.behavior==1?'售价':'求购价'):''}}:{{mainData.price==''?'面议':mainData.price}}</view>
				</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
					<view class="font-24 color6 pl-1">{{mainData.city?mainData.city.title:''}}</view>
				</view>
			</view>
		</view>
		
		<!-- 个人详情信息 -->
		<view class="bg-white py-4 px-3 mt-2 d-flex a-center userBox" 
		v-if="mainData.user&&mainData.user[0]&&mainData.user[0].behavior==0||mainData.user[0].behavior==1">
			<image style="overflow: hidden;border-radius: 50%;" :src="mainData.user&&mainData.user[0]&&mainData.user[0].headImgUrl!=''?mainData.user[0].headImgUrl:''" class="userImg"></image>
			<view class="font-26 color2 ml-3 flex-1">
				<view class="pb-3">{{mainData.name?mainData.name:''}}</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
					<view v-if="now < mainData.invalid_time">{{mainData.phone?mainData.phone:''}}</view>
					<view v-else>信息已失效，联系方式不可见</view>
				</view>
			</view>
			<view class="Mcolor line-h-sm Mborder px-3 py-1 rounded" v-if="now < mainData.invalid_time" @click="callPhone">拨打电话</view>
		</view>
		<!-- 公司详情信息 -->
		<view class="bg-white px-3 mt-2" v-if="mainData.user&&mainData.user[0]&&mainData.user[0].behavior==2">
			<view class="d-flex a-center py-4 borderB-f5 color6">
			{{mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].info.company:''}}<view class="tag tagName">已实名认证</view></view>
			<view class="bg-white py-4 d-flex a-start">
				<image :src="mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].headImgUrl:''" class="userImg"></image>
				<view class="ml-3 flex-1">
					<view class="font-26 color2 pb-4 flex-1 d-flex a-center">
						<view class="pr-4">{{mainData.name?mainData.name:''}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
							<view>{{mainData.phone?mainData.phone:''}}</view>
						</view>
					</view>
					<view class="font-24 color6">{{mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].info.passage2:''}}</view>
				</view>
			</view>
			<view class="Mcolor font-24 text-center d-flex a-center j-sa pb-4 ">
				<view class="Mborder rounded10 btn" 
				@click="Router.navigateTo({route:{path:'/pages/secondHand-detailCompany/secondHand-detailCompany?user_no='+mainData.user_no}})">公司主页</view>
				<view class="Mborder rounded10 btn" @click="callPhone">拨打电话</view>
			</view>
		</view>
		
		
		<view class="bg-white mt-3 font-24 colorO text-center py-3">联系我时，请说是在劳务信息小程序上看到的信息哦！</view>
		
		<view class="px-3 bg-white mt-2 tj">
			<view class="font-30 color2 pt-4">相关推荐</view>
			<view class="item d-flex a-center j-sb py-3"  v-for="(item,index) of relationData"
			:key="item.id" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">
						{{item.title}}
					</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>{{item.name}}<view class="tag tagName" v-if="item.user&&item.user[0]&&item.user[0].behavior==2">已实名认证</view></view>
						<view>{{item.city?item.city.title:''}}</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagB" v-if="item.behavior==1">出售</view>
							<view class="tag tagG" v-if="item.behavior==2">求购</view>
							<view class="tag tagO">{{item.label?item.label.title:''}}</view>
							<view class="tag tagR" v-if="item.top>0">置顶</view>
							<view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view>
						</view>
						<view class="font-22 color9">{{Utils.formatMsgTime(item.update_time)}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!-- 分享 -->
		<view class="bg-mask" v-show="share_show">
			<view class="bg-white rounded20-T color2 font-28 text-center position-absolute bottom-0 left-0 right-0">
				<view class="py-4">分享给</view>
				<view class="d-flex a-center j-sb share m-a mb-5 pt-3">
					<view class="font-26 pb-1"><image src="../../static/images/sharel-icon.png" class="img1"></image><view>微信好友</view></view>
					<view class="font-26 pb-1" @click="Router.navigateTo({route:{path:'/pages/secondHand-share/secondHand-share?id='+mainData.id}})"><image src="../../static/images/sharel-icon1.png" class="img2"></image><view>微信朋友圈</view></view>
				</view>
				<view class="font-26 py-3 bg-colorf5 mt-5 z-index100" @click="changeShare()">取消</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				share_show:false,
				id:'',
				mainData:{},
				currIndex:0,
				Utils:this.$Utils,
				now:0,
				relationData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.now = Date.parse(new Date()) / 1000;
			
			self.$Utils.loadAll(['getMainData','getQrCode'], self);
		},
		
		methods: {
			
			getQrCode() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.id,
					page: 'pages/secondHand-detail/secondHand-detail',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('secondHandQr',res.info.url)
					}
					self.$Utils.finishFunc('getQrCode');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			callPhone(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.mainData.phone
				})
			},
			
			changeShare(){
				const self = this;
				self.share_show = !self.share_show;
			},
			
			collect() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '收藏成功',
					relation_id: self.mainData.id,
					relation_table: 'Message',
					user_no: uni.getStorageSync('user_info').user_no,
					behavior:2
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;
			
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'=',
						//info:['headImgUrl']
					},
					label:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					},
					city:{
						tableName:'Label',
						middleKey:'location',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					},
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Message'
						},
						condition: '='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.getRelationData()
						//self.mainData.invalid_time = self.$Utils.timeto(self.mainData.invalid_time*1000,'ymd')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			getRelationData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				};
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:0,
					type:1
				};
				postData.order  = {
					listorder:'desc'
				};
				if(self.mainData.user[0].behavior==2){
					postData.searchItem.user_no = self.mainData.user_no
				}else{
					postData.getBefore = {
						relation:{
							tableName:'User',
							middleKey:'user_no',
							key:'user_no',
							searchItem:{
								behavior:['in',[2]]
							},
							condition:'in'
						}
					}
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						//info:['headImgUrl']
					},
					label:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					},
					city:{
						tableName:'Label',
						middleKey:'location',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					},
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Message'
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.relationData = res.info.data;
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			preview(index){
				const self = this;
				var urls = [];
				for (var i = 0; i < self.mainData.mainImg.length; i++) {
					urls.push(self.mainData.mainImg[i].url)
				};
				uni.previewImage({
					current:index,
					urls:urls
				})
			},
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.banner{height: 505rpx;position: relative;}
.swiper-sign{background-color: #434343;position: absolute;bottom: 30rpx;right: 30rpx;}
.icon1{width: 31rpx;height: 23rpx;}
.icon2{width: 33rpx;height: 34rpx;}
.icon3{width: 36rpx;height: 34rpx;}
.icon4{width: 21rpx;height: 25rpx;}
.icon5{width: 20rpx;height: 26rpx;margin-right: 10rpx;}

.userImg{width: 100rpx;height: 100rpx;}

.tj .item image{width: 180rpx;height: 180rpx;}
.tj .itemCon .tit{width: 480rpx;line-height: 1.2;}
.tj .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.btn{width: 260rpx;line-height: 50rpx;}

.share{width: 440rpx;}
.share>view{display: flex;flex-direction: column;align-items: center;}
.img1{width: 83rpx;height: 70rpx;margin-bottom: 35rpx}
.img2{width: 76rpx;height: 77rpx;margin-bottom: 35rpx}
</style>
