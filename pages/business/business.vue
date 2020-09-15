<template>
	<view class="h-100 d-flex flex-column">
		<view class="head px-3 pb-1 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar +'px'}"
		 @click="checkLogin">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		<!--  -->
		<!-- :style="{marginTop:statusBar+64 +'px'}" -->
		<view class="d-flex h-100 con flex-1">
			<!-- 左边 -->
			<view class="list font-28 color2 overflow-a">
				<view class="d-flex a-center j-center" :class="listCurr==-2?'on':''" @click="changeList(-2)">
					<image src="../../static/images/businessl-icon.png" class="icon2"></image>搜索
				</view>
				<view :class="listCurr==-1?'on':''" @click="changeList(-1)">为您推荐</view>
				<view v-for="(item,index) of menuData" :key="item.id" :class="listCurr==index?'on':''" @click="changeList(index)">{{item.title}}</view>
			</view>
			<!-- 右边 -->
			<scroll-view scroll-y="true" @scrolltolower="Bottom()" class="flex-1 bg-white h-100">

				<view class="search p-2" v-show="listCurr==-2">
					<view class="font-28 color2 d-flex border-e1 a-center px-2 ss">
						<input type="text" placeholder="请输入关键词搜索" v-model="keywords" />
						<view class="pl-2 borderL-f5" @click="search">搜索</view>
					</view>

					<view class="font-22 color9 d-flex flex-wrap">
						<view class="py-2 mr-1" v-for="(item,index) of historyData" :key="item.id" @click="clickSearch(index)">{{item.result}}</view>
					</view>
				</view>
				<view class="font-28 color2 d-flex a-center j-sb borderB-e1 z-index1000 nav" v-show="listCurr!=-2&&listCurr!=-1">
					<view class="item d-flex a-center j-center" :class="navCurr==1?'on':''" @click="changeCurr(1)">分类
						<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==1"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view>
					<view class="item d-flex a-center j-center" :class="navCurr==2?'on':''" @click="changeCurr(2)">销售区域
						<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==2"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view>
					<view class="item d-flex a-center j-center" :class="navCurr==3?'on':''" @click="changeCurr(3)">所在地
						<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==3"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view>
				</view>
				<view class="rounded shadow m-2 px-2  pb-3" v-for="(item,index) of mainData" :key="item.id">
					<view class="Mcolor font-30 py-3">{{item.title}}</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
						<view><text class="borderR-e1 pr-2 mr-2">{{item.name}}</text>{{item.phone}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
							<view class="font-24 color6 pl-1">{{item.city?item.city.title:''}}</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">{{item.description}}</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="d-flex a-center flex-wrap" v-if="item.mainImg.length>0">
							<image v-for="(c_item,c_index) of item.mainImg" :key="c_index" :src="item.url" class="img"></image>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">销售区域：</view>
						<view class="color2 mr-1" :key="c_item.id" v-for="(c_item,c_index) in item.relation">{{c_item.relation_two}}</view>
					</view>
					<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
						<view class="btn Mborder rounded" @click="collect(index)">
							{{item.log&&item.log.length>0&&item.log[0].status==1?'已收藏':'收藏'}}
						</view>
						<view class="btn Mborder rounded ml-5" @click="callPhone(index)">拨打电话</view>
					</view>
					<view class="font-22 color2 mt-2 btn1" @click="Router.navigateTo({route:{path:'/pages/user-opinion/user-opinion'}})">不实信息投诉</view>
				</view>
			</scroll-view>


		</view>

		<view class="oh bg-mask position-fixed top-0 left-0 right-0" @click="closeMask" :style="{marginTop:statusBar+63 +'px'}"
		 v-show="menuShow||cityShow||saleCityShow">
			<!-- 分类 -->

		</view>
		<view :style="{marginTop:statusBar+63 +'px'}" style="z-index:41" class="position-fixed top-0 left-0 right-0">
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="menuShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of menuData" :key="item.id" @click="changeMenuIndex(index)" :class="menuIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li" :class="menuIdIndex==index?'on':''" @click="chooseMenuId(index)" v-for="(item,index) of menuData[menuIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="menuIdIndex==index"></image>
					</view>
				</view>
			</view>

			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="cityShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id" @click="changeCityIndex(index)" :class="cityIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li" :class="cityIdIndex==index?'on':''" @click="chooseCityId(index)" v-for="(item,index) of cityData[cityIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="cityIdIndex==index"></image>
					</view>
				</view>
			</view>

			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="saleCityShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id" @click="changeSaleCityIndex(index)" :class="citySaleIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li" :class="citySaleIdIndex==index?'on':''" @click="chooseSaleCityId(index)" v-for="(item,index) of cityData[citySaleIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="citySaleIdIndex==index"></image>
					</view>
				</view>
			</view>
		</view>



		<!-- <view style="height: 100rpx;width: 100%;"></view> -->
		<!-- 底部 -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/secondHand/secondHand'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>二手</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/business/business'}})">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
				<view>商务通</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/labour/labour'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>劳务</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar5.png" mode=""></image>
				<view>我的</view>
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
				listCurr: -1,
				menuData: [],
				cityData: [],
				mainData: [],
				searchItem: {
					thirdapp_id: 2,
					type: 2,
					user_type: 0
				},
				historyData: [],
				keywords: '',
				cityShow: false,
				menuShow: false,
				saleCityShow: false,
				cityIndex: 0,
				cityIdIndex: -1,
				menuIndex: 0,
				menuIdIndex: -1,
				navCurr: -1,
				menuBefore: {},
				citySaleIndex: 0,
				citySaleIdIndex: -1,
				saleCityBefore: {},
				menuBefore: {}
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData', 'getMenuData', 'getCityData', 'getHistoryData'], self);
		},

		methods: {
			
			changeSaleCityIndex(index) {
				const self = this;
				self.citySaleIndex = index
			},
			
			chooseSaleCityId(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.citySaleIdIndex = index;
				self.saleCityBefore = {
					tableName: 'Relation',
					middleKey: 'id',
					key: 'relation_one',
					searchItem: {
						type:['in',[1]],
						relation_two: ['in', [self.cityData[self.citySaleIndex].children[self.citySaleIdIndex].title]]
					},
					condition: 'in'
				};
				self.menuBefore = {};
				delete self.searchItem.location;
				self.navCurr = 2;
				self.saleCityShow = false;
				self.getMainData(true)
			},

			changeMenuIndex(index) {
				const self = this;
				self.menuIndex = index
			},

			chooseMenuId(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.menuIdIndex = index;
				self.listCurr = self.menuIndex;
				self.menuBefore = {
					tableName: 'Relation',
					middleKey: 'id',
					key: 'relation_one',
					searchItem: {
						type:['in',[2]],
						relation_two: ['in', [self.menuData[self.menuIndex].children[self.menuIdIndex].id]]
					},
					condition: 'in'
				};
				self.saleCityBefore = {};
				delete self.searchItem.location;
				self.navCurr = 1;
				self.menuShow = false;
				self.getMainData(true)
			},

			changeCityIndex(index) {
				const self = this;
				self.cityIndex = index
			},

			chooseCityId(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.cityIdIndex = index;
				self.menuBefore = {};
				self.saleCityBefore = {};
				self.searchItem.location = self.cityData[self.cityIndex].children[self.cityIdIndex].id;
				self.navCurr = 3;
				self.cityShow = false;
				self.getMainData(true)
			},

			closeMask() {
				const self = this;
				self.menuShow = false;
				self.cityShow = false;
			},

			Bottom() {
				console.log('onReachBottom')
				const self = this;
				if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
					self.paginate.currentPage++;
					self.getMainData()
				};
			},

			clickSearch(index) {
				const self = this;
				self.keywords = self.historyData[index].result;
				if (self.keywords == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('数据有误', 'none')
				} else {
					self.getMainData(true)
				}
			},

			search() {
				const self = this;
				uni.setStorageSync('canClick', true);
				if (self.keywords == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入关键词搜索', 'none')
				} else {
					self.addHistory()
					self.getMainData(true)
				}
			},

			getHistoryData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					type: 2,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.historyData = res.info.data
					}
					self.$Utils.finishFunc('getHistoryData');
				};
				self.$apis.logGet(postData, callback);
			},

			addHistory() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 2,
					result: self.keywords
				};
				postData.noShowLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.historyData.push({
							result: self.keywords,
							id: res.info.id
						});
					}
				};
				self.$apis.logAdd(postData, callback);
			},

			changeCurr(type) {
				const self = this;
				if (type == 1) {
					self.menuShow = true
					self.saleCityShow = false;
					self.cityShow = false;
				} else if (type == 2) {
					self.menuShow = false
					self.saleCityShow = true;
					self.cityShow = false;
				} else if (type == 3) {
					self.menuShow = false
					self.saleCityShow = false;
					self.cityShow = true;
				}
			},

			callPhone(index) {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.mainData[index].phone
				})
			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				if (self.listCurr == -1) {
					postData.searchItem.top = 1
				} else if (self.listCurr != -2) {
					postData.getBefore = {
						relation: {
							tableName: 'Relation',
							middleKey: 'id',
							key: 'relation_one',
							searchItem: {
								relation_two_table: ['in', [self.menuData[self.listCurr].id]]
							},
							condition: 'in'
						}
					}
				};
				if (JSON.stringify(self.menuBefore) != '{}') {
					postData.getBefore.relation1 = self.menuBefore
				};
				if (JSON.stringify(self.saleCityBefore) != '{}') {
					postData.getBefore.relation2 = self.saleCityBefore
				};
				if (self.keywords != '') {
					postData.keywords = self.keywords
				};
				postData.getAfter = {
					relation: {
						tableName: 'Relation',
						middleKey: 'id',
						key: 'relation_one',
						searchItem: {
							status: 1,
							type: 1
						},
						condition: '=',
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
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.searchCard(postData, callback);
			},

			changeList(i) {
				const self = this;
				self.mainData = [];
				self.listCurr = i;
				if (self.listCurr != -2) {
					self.getMainData(true)
				}
			},


			collect(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.mainData[index].log.length == 0) {
					self.addGoodLog(index)
				} else {
					self.updateGoodLog(index)
				};
			},

			addGoodLog(index) {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '收藏成功',
					relation_id: self.mainData[index].id,
					relation_table: 'Message',
					user_no: uni.getStorageSync('user_info').user_no,
					behavior: 2
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData[index].log.push({
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
						id: self.mainData[index].log[0].id
					},
					data: {
						status: -self.mainData[index].log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData[index].log[0].status = -self.mainData[index].log[0].status;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

			checkLogin() {
				const self = this;
				if (!uni.getStorageSync('user_info') || uni.getStorageSync('user_info').headImgUrl == '') {
					uni.showModal({
						title: '提示',
						content: '您未登录，是否立即登录',
						success(res) {
							if (res.confirm) {
								self.Router.redirectTo({
									route: {
										path: '/pages/user/user'
									}
								})
							}
						}
					})
				} else {
					self.Router.navigateTo({
						route: {
							path: '/pages/business-publish/business-publish'
						}
					})
				}
			},

			getCityData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					type: 2
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.cityData = res.info.data;
					}
					self.$Utils.finishFunc('getCityData');
				};
				self.$apis.labelGet(postData, callback);
			},

			getMenuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					type: 4
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.menuData = res.info.data;
					}
					self.$Utils.finishFunc('getMenuData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	}
</script>

<style>
	page {
		height: 100%;
	}

	.bg-f5 {
		background-color: #f5f5f5;
	}

	.head{line-height: 80rpx;}
	.icon1 {
		width: 23rpx;
		height: 23rpx;
		margin-right: 10rpx;
	}

	.icon2 {
		width: 24rpx;
		height: 24rpx;
		margin-right: 10rpx;
	}

	.icon3 {
		width: 21rpx;
		height: 25rpx;
	}

	.img {
		width: 106rpx;
		height: 106rpx;
		margin-top: 20rpx;
		margin-right: 20rpx;
	}

	.imgBox .img:nth-child(3n) {
		margin-right: 0;
	}

	/* .listBox{width: 180rpx;} */
	.list {
		background-color: #D8EDFE;
		width: 180rpx;
		line-height: 100rpx;
		text-align: center;
		top: 0;
		height: 100%;
	}

	.list view {
		width: 100%;
	}

	.list .on {
		background-color: #fff;
	}

	.btn {
		width: 120rpx;
		line-height: 40rpx;
	}

	.shadow {
		box-shadow: 0 0rpx 16rpx rgba(114, 130, 138, 0.2) !important;
	}

	.con {
		overflow-y: scroll;
	}

	.search input {
		text-align: left;
		color: #222;
		font-size: 28rpx;
		flex: 1;
	}

	.search .ss {
		height: 80rpx;
	}

	.w3 {
		width: 33.33%;
	}

	.overflow-a {
		overflow-y: auto;
	}

	.footer {
		position: static;
	}

	.nav image {
		width: 18rpx;
		height: 9rpx;
		margin-left: 8rpx;
	}

	.nav .item {
		width: 33%;
		line-height: 90rpx;
		text-align: center;
	}

	.nav .on {
		position: relative;
		color: #51A9E9;
	}

	.nav .on::before {
		content: '';
		width: 100%;
		height: 2rpx;
		background-color: #51A9E9;
		position: absolute;
		bottom: 0;
		left: 0;
	}

	.classfiy .left {
		width: 180rpx;
		background-color: #f5f5f5;
	}

	.classfiy .left .on {
		color: #55AAE9;
		position: relative;
		background: #fff;
	}

	.classfiy .left .on::before {
		content: '';
		background-color: #55AAE9;
		height: 100%;
		width: 4rpx;
		position: absolute;
		top: 0;
		left: 0;
	}

	.icon5 {
		width: 36rpx;
		height: 26rpx;
	}

	.classfiy .right .li {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx;
		border-bottom: 1px solid #f5f5f5;
	}

	.classfiy .right .on {
		color: #51A9E9;
	}
	
	.btn1{width: 155rpx;padding: 10rpx;border: 1px solid #e1e1e1;}
</style>
