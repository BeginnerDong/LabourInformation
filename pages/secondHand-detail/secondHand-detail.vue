<template>
	<view>
		<!-- banner -->
		<view class="banner" v-if="mainData.mainImg&&mainData.mainImg.length>0">
			<swiper class="swiper-box h-100" circular autoplay="autoplay"  interval="4000" :current="currIndex"   @animationfinish="changeCurrent">
				<block v-for="(item,index) in mainData.mainImg" :key="index">
					<swiper-item class="swiper-item">
						<image mode="aspectFill" :src="item.url" @click="preview(index)"/>
					</swiper-item>
				</block>
			</swiper>
			<view class="swiper-sign colorf font-28 line-h-md px-1 rounded10">{{currIndex+1}}/{{mainData.mainImg?mainData.mainImg.length:0}}</view>
		</view>
		
		<view class="px-3 bg-white line-h">
			<view class="font-32  color3 pt-4  line-h-md">{{mainData.title?mainData.title.substr(0,100):''}}</view>
			<!-- <view class="d-flex a-center">
				<image src="../../static/images/detailsl-icon.png" class="icon1"></image>
				<view class="font-24 color6 pl-1 py-4">{{mainData.view_count}}</view>
			</view> -->
			<view class="d-flex a-center j-sb h-100 py-4 borderB-f5">
				<view class="font-22 d-flex a-center">
					<!-- <view class="tag tag1" v-if="mainData.keywords&&mainData.keywords.length>0&&mainData.keywords[0].length>0" v-for="(c_item,c_index) of mainData.keywords" :key="c_index">{{c_item}}</view> -->
					<view class="tag tagB" v-if="mainData.behavior==1">出售</view>
					<view class="tag tagG" v-if="mainData.behavior==2">求购</view>
					<view class="tag tagO">{{mainData.label?mainData.label.title:''}}</view>
					<view class="tag tagP">{{mainData.price&&mainData.price!=''?mainData.price:'面议'}}</view>
				</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
					<view class="font-24 color6 pl-1">{{mainData.city?mainData.city.title:''}}</view>
				</view>
			</view>
			<view class="color6 font-24 d-flex j-sb a-start py-4">
				<view class="line-h">
					<view class="pb-2">首发日期：{{mainData.create_time?mainData.create_time:''}}</view>
					<view>更新日期：{{mainData.update_time&&mainData.update_time!=mainData.create_time?mainData.update_time:'-'}}</view>
					<!-- <view v-if="now < mainData.invalid_time">更新时间：{{mainData.update_time?mainData.update_time:''}}</view> -->
					<!-- <view class="Rcolor" v-else>本条信息已失效</view> -->
				</view>
				<view class="d-flex a-center" style="margin-top: 15rpx;">
					<view class="d-flex a-center"  @click="Utils.stopMultiClick(collect)">
						<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/detailsl-icon1.png':'../../static/images/detailsl-icon5.png'" 
						class="icon6"></image>
						<view class="pl-1">{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'收藏'}}</view>
					</view>
					<view class="d-flex a-center ml-5" @click="changeShare()">
						<image src="../../static/images/detailsl-icon2.png" class="icon2"></image>
						<view class="pl-1">分享</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 个人详情信息 -->
		<view class="bg-white px-3 mt-2" v-if="mainData.user&&mainData.user[0]&&mainData.user[0].behavior==0||mainData.user[0].behavior==1">
			<view class="d-flex a-center py-4 borderB-f5 color2 font-w">个人发布</view>
			<view class="bg-white py-4  d-flex a-center userBox">
				<image style="overflow: hidden;border-radius: 50%;" :src="mainData.user&&mainData.user[0]&&mainData.user[0].headImgUrl!=''?mainData.user[0].headImgUrl:''" class="userImg"></image>
				<view class="font-26 color2 ml-3 flex-1">
					<view class="pb-3">{{mainData.name?mainData.name:''}}</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
						<view v-if="now < mainData.invalid_time">{{mainData.phone?mainData.phone:''}}</view>
						<view v-if="now > mainData.invalid_time">信息已超过7天，联系方式不可见</view>
					</view>
				</view>
				<view class="Mcolor line-h-sm Mborder px-3 py-1 rounded" v-if="now < mainData.invalid_time" @click="callPhone">拨打电话</view>
			</view>
		</view>
		<!-- {{mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].info.company:''}}<view class="tag tagName">二手商</view> -->
		
		<!-- 公司详情信息 -->
		<view class="bg-white px-3 mt-2" v-if="mainData.user&&mainData.user[0]&&mainData.user[0].behavior==2">
			<view class="d-flex a-center py-4 borderB-f5 color2 font-w">二手商发布
			<!-- {{mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].info.company:''}}<view class="tag tagName">二手商</view> --></view>
			<view class="bg-white py-4 d-flex a-start">
				<image :src="mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].headImgUrl:''" class="userImg"></image>
				<view class="ml-3 flex-1">
					<view class="font-26 color2 mb-2 flex-1 d-flex a-center" style="width: 58%;">
						<view class="pr-4">{{mainData.name?mainData.name:''}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon4.png" class="icon5"></image>
							<view class="">{{mainData.phone?mainData.phone:''}}</view>
						</view>
					</view>
					<view class="font-24 color6">{{mainData.user&&mainData.user[0]&&mainData.user[0]?mainData.user[0].info.passage2.substr(0,80):''}}</view>
				</view>
			</view>
			<view class="Mcolor font-24 text-center d-flex a-center j-sa pb-4 ">
				<view class="Mborder rounded10 btn" 
				@click="Router.navigateTo({route:{path:'/pages/secondHand-detailCompany/secondHand-detailCompany?user_no='+mainData.user_no}})">更多二手设备</view>
				<view class="Mborder rounded10 btn" @click="callPhone">拨打电话</view>
			</view>
		</view>
		
		
		<view class="bg-white mt-2 font-24 colorO text-center py-3">联系我时，请说明是在“桥隧之家”小程序上看到的信息哦！</view>
		
		<view class="px-3 bg-white mt-2 tj">
			<view class="font-30 color2 pt-4">相关推荐</view>
			<view class="item d-flex a-center j-sb py-3"  v-for="(item,index) of relationData"
			:key="item.id" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
				<image mode="aspectFill" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:(item.behavior==2?'../../static/images/qiugou.jpg':'')" ></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit"  style="height: 84rpx;">
						{{item.title}}
					</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-2 line-h">
						<view class="d-flex a-center">{{item.name}}<view class="tag tagName" v-if="item.user&&item.user[0]&&item.user[0].behavior==2">二手商</view></view>
						<view>{{item.city?item.city.title:''}}</view>
					</view>
					<view class="d-flex a-center j-sb  mt-2">
						<view class="font-22 d-flex a-center">
							<!-- <view class="tag tag1" v-if="item.keywords&&item.keywords.length>0&&item.keywords[0].length>0" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view> -->
							<view class="tag tagB" v-if="item.behavior==1">出售</view>
							<view class="tag tagG" v-if="item.behavior==2">求购</view>
							<view class="tag tagO">{{item.label?item.label.title:''}}</view>
							<!-- <view class="tag tagR" v-if="item.description!=''">{{item.description}}</view> -->
							<!-- <view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view> -->
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
					<button open-type="share" class="font-26 pb-1">
						<image src="../../static/images/sharel-icon.png" class="img1"></image>
						<view>微信好友</view>
					</button>
					<view class="font-26 pb-1" style="line-height: 1.4;" @click="Router.navigateTo({route:{path:'/pages/secondHand-share/secondHand-share?id='+mainData.id}})">
						<image src="../../static/images/sharel-icon1.png" class="img2"></image>
						<view>微信朋友圈</view>
					</view>
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
			if(options.scene){
				self.id = decodeURIComponent(options.scene)
			}else{
				self.id = options.id;
			}
			self.now = Date.parse(new Date()) / 1000;
			self.$Utils.loadAll(['getMainData','getQrCode'], self);
		},
		onShow() {
			const self = this;
			self.currIndex = 0
		},
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title:self.mainData.title,
					path: '/pages/secondHand-detail/secondHand-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
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
					path: '/pages/secondHand-detail/secondHand-detail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
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
						self.mainData.create_time = self.mainData.create_time.substr(0,10)
						self.mainData.update_time = self.mainData.update_time.substr(0,10)
						
						self.mainData.keywords = self.mainData.keywords.split(',');
						self.mainData.user[0].passage2 = self.mainData.user[0].passage2
						
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
					type:1,
					behavior:self.mainData.behavior
				};
				postData.order  = {
					update_time:'desc'
				};
				if(self.mainData.user[0].behavior==2){
					delete postData.searchItem.behavior;
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
						for (var i = 0; i < self.relationData.length; i++) {
							self.relationData[i].keywords = self.relationData[i].keywords.split(',')
						}
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
.icon6{width: 36rpx;height: 36rpx;}
.userImg{width: 100rpx;height: 100rpx;}

.tj .item image{width: 180rpx;height: 175rpx;}
.tj .itemCon .tit{width: 480rpx;}
.tj .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.btn{width: 260rpx;line-height: 50rpx;}

.share{width: 440rpx;line-height: 1.4;}
button{line-height: 1.4;}
.share>view{display: flex;flex-direction: column;align-items: center;}
.share>button{display: flex;flex-direction: column;align-items: center;background-color: #fff;}
.img1{width: 84rpx;height: 77rpx;margin-bottom: 35rpx}
.img2{width: 77rpx;height: 77rpx;margin-bottom: 35rpx}
</style>
