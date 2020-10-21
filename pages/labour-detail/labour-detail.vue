<template>
	<view>
		<!-- banner -->
		<view class="banner" v-if="mainData.mainImg&&mainData.mainImg.length>0">
			<swiper class="swiper-box h-100" autoplay="autoplay" interval="4000" :current="currIndex" @change="changeCurrent">
				<block v-for="(item,index) in mainData.mainImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" @click="preview(index)"/>
					</swiper-item>
				</block>
			</swiper>
			<view class="swiper-sign colorf font-22 line-h-md px-1 rounded10">{{currIndex+1}}/{{mainData.mainImg?mainData.mainImg.length:0}}</view>
		</view>
		
		<view class="px-3 bg-white">
			<view class="font-30 color3 pt-5">
				{{mainData.title?mainData.title:''}}
			</view>
			<view class="d-flex a-center">
				<image src="../../static/images/detailsl-icon.png" class="icon1"></image>
				<view class="font-24 color6 pl-1 py-3">{{mainData.view_count}}</view>
			</view>
			<view class="color6 font-24 d-flex j-sb a-start pb-3 borderB-f5">
				<view class="line-h">
					<view class="pb-2">首发时间：{{mainData.create_time?mainData.create_time:''}}</view>
					<view>更新时间：{{mainData.update_time&&mainData.update_time!=mainData.create_time?mainData.update_time:'-'}}</view>
					<!-- <view v-if="now < mainData.invalid_time">更新时间：{{mainData.update_time?mainData.update_time:''}}</view>
					<view class="Rcolor" v-else>本条信息已失效</view> -->
				</view>
				<view class="d-flex a-center">
					<view class="d-flex a-center"  @click="Utils.stopMultiClick(collect)">
						<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/detailsl-icon1.png':'../../static/images/detailsl-icon5.png'" class="icon2"></image>
						<view class="pl-1">{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'收藏'}}</view>
					</view>
					<view class="d-flex a-center ml-5" @click="changeShare()">
						<image src="../../static/images/detailsl-icon2.png" class="icon2"></image>
						<view class="pl-1">分享</view>
					</view>
				</view>
			</view>
			<view class="d-flex a-center j-sb h-100 pt-4 pb-4">
				<view class="font-22 d-flex a-center">
					
					<view class="tag tagB" v-if="mainData.behavior==1">招工人</view>
					<view class="tag tagB" v-if="mainData.behavior==2">招队伍</view>
					<view class="tag tagG" v-if="mainData.behavior==3">工人找活</view>
					<view class="tag tagG" v-if="mainData.behavior==4">队伍找活</view>
					<view class="tag tagY" v-if="mainData.invalid_time<now">信息已失效</view>
					<view v-if="mainData.price!=''"><text class="tag tagR">介绍费</text><text class="tag tagO">{{mainData.price}}</text></view>
				</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
					<view class="font-24 color6 pl-1">{{mainData.city?mainData.city.title:''}}</view>
				</view>
			</view>
			<view class="font-22 Rcolor pt-2 pb-4" v-if="mainData.price!=''">信息发布者愿意支付给帮忙成功介绍者介绍费（平台不提供担保）</view>
		</view>
		
		<!-- 个人详情信息 -->
		<view class="bg-white px-3 mt-2">
			<view class="py-4 d-flex a-center userBox">
				<image :src="mainData.user&&mainData.user[0]&&mainData.user[0].headImgUrl!=''?mainData.user[0].headImgUrl:''" class="userImg"></image>
				<view class="font-26 color2 ml-3 flex-1">
					<view class="pb-3">{{mainData.name?mainData.name:''}}</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
						<view v-if="now < mainData.invalid_time">{{mainData.phone?mainData.phone:''}}</view>
						<view v-else>信息已超过7天，联系方式不可见</view>
					</view>
				</view>
				<view class="Mcolor line-h-sm Mborder px-3 py-1 rounded" v-if="now < mainData.invalid_time" @click="callPhone">拨打电话</view>
			</view>
			<!-- <view class="line-h-md py-4 font-24 color6 dashedBorder">
				<view v-if="mainData.behavior==1" v-for="(c_item,c_index) in mainData.passage_array" :key="index">{{c_item.name}}，{{c_item.num}}名，
				{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
				<view v-if="mainData.behavior==2" v-for="(c_item,c_index) in mainData.passage_array" :key="index">{{c_item.name}}，{{c_item.num}}支队伍，
				{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
				<view v-if="mainData.behavior==3">求职岗位：{{mainData.description}},期待工资：{{mainData.salary}}</view>
				<view v-if="mainData.behavior==4">我能承包：<span :key="index" v-for="(c_item,c_index) in mainData.passage_array">{{c_item.name}}</span></view>
			</view> -->
		</view>
		
		<view class="bg-white mt-2 font-24 colorO text-center py-3">联系我时，请说明是在“桥隧之家”小程序上看到的信息哦！</view>
		
		<view class="py-5 font-26 Mcolor text-center" @click="checkLogin">我也要发布招工求职信息</view>
		
		
		<!-- 分享 -->
		<view class="bg-mask" v-show="share_show">
			<view class="bg-white rounded20-T color2 font-28 text-center position-absolute bottom-0 left-0 right-0">
				<view class="py-4">分享给</view>
				<view class="d-flex a-center j-sb share m-a mb-5 pt-3">
					<button open-type="share" class="font-26 pb-1"><image src="../../static/images/sharel-icon.png" class="img1"></image><view>微信好友</view></button>
					<view class="font-26 pb-1" @click="Router.navigateTo({route:{path:'/pages/labour-share/labour-share?id='+mainData.id}})"><image src="../../static/images/sharel-icon1.png" class="img2"></image><view>微信朋友圈</view></view>
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
				Utils:this.$Utils,
				now:0,
				mainData:{},
				currIndex:0
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.scene){
				self.id = decodeURIComponent(options.scene)
			}else{
				self.id = options.id;
			}
			self.now = Date.parse(new Date()) / 1000;
			self.$Utils.loadAll(['getMainData','getQrCode'], self);
		},
		
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title:self.mainData.title,
					path: '/pages/labour-detail/labour-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title:self.mainData.title,
					path: '/pages/labour-detail/labour-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}
		},
		
		methods: {
			
			changeCurrent(e){
				console.log(e)
				const self = this;
				self.currIndex = e.detail.current
			},
			
			getQrCode() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.id,
					page: 'pages/labour-share/labour-share',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('labourQr',res.info.url)
					}
					self.$Utils.finishFunc('getQrCode');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			checkLogin(){
				const self = this;
				if(!uni.getStorageSync('user_info')||uni.getStorageSync('user_info').headImgUrl==''){
					uni.showModal({
						title:'提示',
						content:'您未登录，是否立即登录',
						success(res) {
							if(res.confirm){
								self.Router.reLaunch({route:{path:'/pages/user/user'}})
							}
						}
					})
				}else{
					self.Router.redirectTo({route:{path:'/pages/labour-publish/labour-publish'}})
				}
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
							status:1
						},
						condition:'=',
						//info:['headImgUrl']
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
						//self.getRelationData()
						//self.mainData.invalid_time = self.$Utils.timeto(self.mainData.invalid_time*1000,'ymd')
						self.mainData.create_time = self.mainData.create_time.substr(0,10)
						self.mainData.update_time = self.mainData.update_time.substr(0,10)
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
.icon1{width: 31rpx!important;height: 23rpx!important;}
.icon2{width: 33rpx!important;height: 34rpx!important;}
.icon3{width: 36rpx!important;height: 34rpx!important;}
.icon4{width: 21rpx!important;height: 25rpx!important;}
.icon5{width: 20rpx!important;height: 26rpx;margin-right: 10rpx!important;}

.userImg{width: 100rpx;height: 100rpx;}

.tj .item image{width: 180rpx;height: 180rpx;}
.tj .itemCon .tit{width: 480rpx;line-height: 1.2;}
.tj .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.btn{width: 260rpx;line-height: 50rpx;}

.share{width: 440rpx;}
.share>view{display: flex;flex-direction: column;align-items: center;}
.share>button{display: flex;flex-direction: column;align-items: center;background-color: #fff;}
.img1{width: 83rpx;height: 70rpx;margin-bottom: 35rpx}
.img2{width: 76rpx;height: 77rpx;margin-bottom: 35rpx}

.tagR{margin-right: 0;}
.dashedBorder{border-top: 1px dashed #e1e1e1;}
</style>
