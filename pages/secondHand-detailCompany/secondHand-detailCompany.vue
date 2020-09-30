<template>
	<view>
		
		<view class="headBox px-3 bg-mcolor font-32 colorf z-index100" :style="{paddingTop:statusBar +'px'}">
			<view class="d-flex a-center">
				<view class="py-1 pr-2" @click="Router.back(1)">
					<image src="../../static/images/back.png" class="back"></image>
				</view>
				<image src="../../static/images/logo.png" class="logo"></image>
			</view>
		</view>
		
		<view class="head position-relative">
			<image :src="info.mainImg&&info.mainImg[0]?info.mainImg[0].url:''" mode=""></image>
			<view class="txt px-3 font-32 colorf d-flex a-center">
				<view class="name">{{info.company?info.company:''}}</view>
				<view class="tag tagName1 ml-2">已实名认证</view>
			</view>
		</view>

		<view class="px-3 bg-white">
			<!-- <view class="font-26 color9 py-3 borderB-e1 position-relative">{{info.passage1?info.passage1:''}}
				<image src="../../static/images/labor%20releasel-icon1.png" class="icon1"></image>
			</view> -->
			<view class="bg-white py-4 d-flex a-start">
				<view>
					<image :src="userData.headImgUrl?userData.headImgUrl:''" class="userImg"></image>
					<!-- <view class="Rcolor text-center font-18 pt-1">热度98561</view> -->
				</view>
				<view class="ml-3 flex-1">
					<view class="font-26 font-w color2 mb-2 flex-1 d-flex a-center border-e1" style="width: 58%;">
						<view class="pr-4">{{info.name?info.name:''}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon4.png" class="icon2"></image>
							<view>{{info.phone?info.phone:''}}</view>
						</view>
					</view>
					<view class="font-24 color6">{{info.passage2?info.passage2:''}}</view>
				</view>
			</view>
			<view class="Mcolor font-24 text-center d-flex a-center j-sa pb-4 ">
				<view class="Mborder rounded10 btn">{{userData.log&&userData.log.length>0&&userData.log[0].status==1?'已收藏':'我要收藏'}}</view>
				<view class="Mborder rounded10 btn" @click="changeShare()">我要分享</view>
				<view class="Mborder rounded10 btn" @click="callPhone">拨打电话</view>
			</view>
		</view>

		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 shadow-sm mt-2 bg-white nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">求购</view>
		</view>

		<!-- 列表 -->
		<view class="list">
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" v-for="(item,index) of relationData" :key="item.id"
			 :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">
						{{item.title}}
					</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view class="d-flex a-center">{{item.name}}
							<view class="tag tagName" v-if="item.user&&item.user[0]&&item.user[0].behavior==2">已实名认证</view>
						</view>
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

		<view class="bg-mask" v-show="share_show">
			<view class="bg-white rounded20-T color2 font-28 text-center position-absolute bottom-0 left-0 right-0">
				<view class="py-4">分享给</view>
				<view class="d-flex a-center j-sb share m-a mb-5 pt-3">
					<button open-type="share" class="font-26 pb-1">
						<image src="../../static/images/sharel-icon.png" class="img1"></image>
						<view>微信好友</view>
					</button>
					<view class="font-26 pb-1" @click="Router.navigateTo({route:{path:'/pages/secondHand-detailCompany-share/secondHand-detailCompany-share?user_no='+user_no}})">
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
	const app = getApp();
	export default {
		data() {
			return {
				Router: this.$Router,
				statusBar: app.globalData.statusBar,
				navCurr: 1,
				userData: {},
				info: {},
				relationData: [],
				share_show: false,
				Utils: this.$Utils,
				user_no: '',
				searchItem:{
					thirdapp_id: 2,
					user_type: 0,
					type: 1
				},
			}
		},

		onLoad(options) {
			const self = this;
			if (options.scene) {
				self.user_no = decodeURIComponent(options.scene)
			} else {
				self.user_no = options.user_no;
			}
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserData', 'getQrCode'], self);
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getRelationData()
			};
		},

		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: self.info.company ? self.info.company : '',
					path: '/pages/secondHand-detailCompany/secondHand-detailCompany?user_no=' + self.userData.user_no, //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功

						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			} else {
				return {
					title: self.info.company ? self.info.company : '',
					path: '/pages/secondHand-detailCompany/secondHand-detailCompany?user_no=' + self.userData.user_no, //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
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

			getQrCode() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.user_no,
					page: 'pages/secondHand-detailCompany/secondHand-detailCompany',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('userQr', res.info.url)
					}
					self.$Utils.finishFunc('getQrCode');
				};
				self.$apis.getQrCode(postData, callback);
			},

			changeShare() {
				const self = this;
				self.share_show = !self.share_show;
			},


			callPhone() {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.userData.info.phone
				})
			},

			collect() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.userData.log.length == 0) {
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
					relation_id: self.userData.user_no,
					relation_table: 'User',
					user_no: uni.getStorageSync('user_info').user_no,
					behavior: 5
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userData.log.push({
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
						id: self.userData.log[0].id
					},
					data: {
						status: -self.userData.log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.userData.log[0].status = -self.userData.log[0].status;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

			changeNav(i) {
				const self = this;
				if (self.navCurr != i) {
					self.navCurr = i;
					if (self.navCurr == 1) {
						delete self.searchItem.behavior
					} else if (self.navCurr == 2) {
						self.searchItem.behavior = 1
					} else if (self.navCurr == 3) {
						self.searchItem.behavior = 2
					};
					self.getRelationData(true)
				}
			},

			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: self.user_no,
					user_type: 0
				};
				postData.getAfter = {
					log: {
						//token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'user_no',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'User'
						},
						condition: '='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						self.info = self.userData.info;
						self.getRelationData()
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.commonUserGet(postData, callback);
			},

			getRelationData(isNew) {
				const self = this;
				if (isNew) {
					self.relationData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					listorder: 'desc'
				};
				postData.searchItem.user_no = self.user_no
				postData.getAfter = {
					user: {
						tableName: 'User',
						middleKey: 'user_no',
						key: 'user_no',
						searchItem: {
							status: 1
						},
						condition: '=',
						//info:['headImgUrl']
					},
					label: {
						tableName: 'Label',
						middleKey: 'menu_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
						info: ['title']
					},
					city: {
						tableName: 'Label',
						middleKey: 'location',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
						info: ['title']
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.relationData.push.apply(self.relationData, res.info.data)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	}
</script>

<style>
	page {
		background-color: #f5f5f5;
	}
	
	.back{width: 20rpx;height: 30rpx;}
	.headBox{position: sticky;top: 0;z-index: 1000;}
	
	.head image {
		height: 240rpx;
	}

	.head .txt {
		line-height: 80rpx;
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		background-color: rgba(0, 0, 0, 0.5);
		
	}
	
	.name{text-shadow:3px 2px 2px rgba(0, 0, 0, 1);}

	.icon1 {
		width: 19rpx!important;
		height: 10rpx!important;
		position: absolute;
		bottom: -10rpx;
		right: 0;
	}

	.icon2 {
		width: 20rpx!important;
		height: 26rpx!important;
		margin-right: 10rpx;
	}

	.userImg {
		width: 100rpx;
		height: 100rpx;
	}

	.btn {
		width: 200rpx;
		line-height: 50rpx;
	}

	.nav image {
		width: 13rpx;
		height: 6rpx;
		margin-left: 8rpx;
	}

	.nav .item {
		width: 33.33%;
		line-height: 90rpx;
		text-align: center;
	}

	.nav .on {
		position: relative;
		color: #51A9E9;
		box-shadow: 0 8px 6px -6px rgba(148, 232, 241, 0.5);
	}

	/* .nav .on::before {
		content: '';
		width: 100%;
		height: 2rpx;
		background-color: #51A9E9;
		position: absolute;
		bottom: 0;
		left: 0;
	} */

	.list .item image {
		width: 180rpx;
		height: 180rpx;
	}

	.list .itemCon .tit {
		width: 480rpx;
		line-height: 1.2;
	}

	.list .itemConL {
		width: 180rpx;
		height: 180rpx;
		line-height: 180rpx;
		background-color: #9DD6FF;
	}

	.share {
		width: 440rpx;
	}

	.share>view {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.share>button {
		display: flex;
		flex-direction: column;
		align-items: center;
		background-color: #fff;
	}

	.img1 {
		width: 83rpx;
		height: 70rpx;
		margin-bottom: 35rpx
	}

	.img2 {
		width: 76rpx;
		height: 77rpx;
		margin-bottom: 35rpx
	}
</style>
