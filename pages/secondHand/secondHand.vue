<template>
	<view :class="menuShow||cityShow?'none':''">
		<view class="headBox">

			<view class="head px-3 pb-1 bg-mcolor font-30 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar +'px'}"
			 @click="checkLogin">
				<image src="../../static/images/businessl-icon1.png" class="add"></image>
				<view>免费发布</view>
			</view>


			<!-- 搜索 -->
			<view class="pt-2 bg-f5" v-if="showNav">
				<view class="border-e1 rounded bg-white font-28 mx-3 py-15 d-flex a-center j-sb ss">
					<input type="text" placeholder="请输入关键词" v-model="title" @confirm="shabi"/>
					<view class="px-4 color6" @click="search">搜索</view>
				</view>
			</view>

			<!-- nav -->
			<view class="font-28 color2 d-flex a-center j-sb  bg-f5 nav" v-if="showNav">
				<view class="item d-flex a-center j-center" :class="searchItem.behavior==1||searchItem.behavior==2?'on':''" 
				@click="changeCurr(1)">{{!searchItem.behavior?'全部':(searchItem.behavior==1?'出售':'求购')}}
					<image src="../../static/images/labor-releasel-icon1.png" v-if="searchItem.behavior==1||searchItem.behavior==2"></image>
					<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
				</view>
				<!-- <view class="item" :class="navCurr==2?'on':''" @click="canClick?changeCurr(2):''">出售</view>
				<view class="item" :class="navCurr==3?'on':''" @click="canClick?changeCurr(3):''">求购</view> -->
				<view class="item d-flex a-center j-center" :class="searchItem.menu_id?'on':''" @click="changeCurr(4)">
					{{menuText!=''?menuText:'分类'}}
					<!-- {{menuIndex>=0&&menuIdIndex<0?menuData[menuIndex].title:(menuIndex>=0&&menuIdIndex>=0?menuData[menuIndex].children[menuIdIndex].title:'分类')}} -->
					<image src="../../static/images/labor-releasel-icon1.png" v-if="searchItem.menu_id"></image>
					<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
				</view>
				<view class="item d-flex a-center j-center" :class="searchItem.location?'on':''" @click="changeCurr(5)">
					{{cityText!=''?cityText:'所在地'}}
					<!-- {{cityIndex>=0&&cityIdIndex<0?cityData[cityIndex].title:(cityIndex>=0&&cityIdIndex>=0?cityData[cityIndex].children[cityIdIndex].title:'所在地')}} -->
					<image src="../../static/images/labor-releasel-icon1.png" v-if="searchItem.location"></image>
					<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
				</view>
			</view>

		</view>



		<!-- 列表 -->
		<view class="list">
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" v-for="(item,index) of mainData" :key="item.id" :data-id="item.id"
			 @click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:(item.behavior==2?'../../static/images/qiugou.jpg':'')"
				 mode="aspectFill"></image>
				<view class="itemCon flex-1 pl-2">
					<view class="color3 font-30 avoidOverflow2 tit" style="height: 84rpx;">
						{{item.title}}
					</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-2 line-h">
						<view class="d-flex a-center">{{item.name}}
							<view class="tag tagName" v-if="item.user&&item.user[0]&&item.user[0].behavior==2">二手商</view>
						</view>
						<view>{{item.city?item.city.title:''}}</view>
					</view>
					<view class="d-flex a-center j-sb  mt-2">
						<view class="font-22 d-flex a-end">
							<!-- <view class="tag tag1" v-if="item.keywords&&item.keywords.length>0&&item.keywords[0].length>0" v-for="(c_item,c_index) of item.keywords"
							 :key="c_index">{{c_item}}</view> -->
							<view class="tag tagB" v-if="item.behavior==1">出售</view>
							<view class="tag tagG" v-if="item.behavior==2">求购</view>
							<view class="tag tagO">{{item.label?item.label.title:''}}</view>
							<view class="tag tagR" v-if="item.description!=''">{{item.description}}</view>
							<!-- <view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view> -->
						</view>
						<view class="font-22 color9">{{Utils.formatMsgTime(item.update_time)}}</view>
					</view>
				</view>
			</view>
		</view>


		<view class="oh bg-mask position-fixed d-flex flex-column" @click="closeMask" :style="{marginTop:showHeight+'px'}" v-show="menuShow||cityShow||sortShow">
			
			<view class="classfiy font-26 color2 line-h  d-flex" v-show="sortShow">
				<view class="sort pl-4" style="width: 100%;background-color: #fff;">
					<view class="li py-3" @click="changeSortIndex(-1)" :class="!searchItem.behavior?'on':''">全部</view>
					<view class="li py-3" @click="changeSortIndex(1)" :class="searchItem.behavior==1?'on':''">出售</view>
					<view class="li py-3" @click="changeSortIndex(2)" :class="searchItem.behavior==2?'on':''">求购</view>
				</view>
				
			</view>
			<!-- 分类 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="menuShow">
				<scroll-view class="left" :style="'height:'+(windowHeight*0.8-217)+'px'" scroll-y="true">
					<view class="li py-3"
					 @click="changeMenuIndex(-1)" :class="menuIndex==-1?'on':''">全部</view>
					<view class="li py-3" v-for="(item,index) of menuData" :key="item.id" @click.stop="changeMenuIndex(index)" :class="menuIndex==index?'on':''">{{item.title}}</view>
				</scroll-view>
				<scroll-view class="right flex-1 bg-white" @click="closeMask" :style="'height:'+(windowHeight*0.8-217)+'px'" scroll-y="true">
					<view class="li py-3" @click="chooseMenuId(-1)" :class="menuIdIndex==-2?'on':''">全部分类</view>
					<view class="li" :class="searchItem.menu_id==item.id?'on':''" @click="chooseMenuId(index)" v-for="(item,index) of menuData[menuIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="searchItem.menu_id==item.id"></image>
					</view>
				</scroll-view>
			</view>

			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="cityShow">
				<scroll-view class="left" :style="'height:'+(windowHeight*0.8-217)+'px'" scroll-y="true">
					<view class="li py-3"
					 @click="changeCityIndex(-1)" :class="cityIndex==-1?'on':''">全部</view>
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id" @click.stop="changeCityIndex(index)" :class="cityIndex==index?'on':''">{{item.title}}</view>
				</scroll-view>
				<scroll-view class="right flex-1 bg-white" @click="closeMask" :style="'height:'+(windowHeight*0.8-217)+'px'" scroll-y="true">
					<view class="li py-3" @click="chooseCityId(-1)" :class="cityIdIndex==-2?'on':''">全部地区</view>
					<view class="li" :class="searchItem.location==item.id?'on':''" @click="chooseCityId(index)" v-for="(item,index) of cityData[cityIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="searchItem.location==item.id"></image>
					</view>
				</scroll-view>
			</view>

			<view class="flex-1" @click="closeMask"></view>

		</view>



		<view class="py-5 font-26 color9 text-center">{{tip}}</view>
		<view style="height: 100rpx;width: 100%;"></view>
		<!-- 底部 -->
		<view class="footer z-index100">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/secondHand/secondHand'}})">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>二手</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/business/business'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
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
				navCurr: 0,
				
				mainData: [],
				now: 0,
				Utils: this.$Utils,
				searchItem: {
					thirdapp_id: 2,
					type: 1,
					user_type: 0
				},
				menuShow: false,
				cityShow: false,
				menuData: [],
				cityData: [],
				menuIndex: -1,
				menuIdIndex: -2,
				cityIndex: -1,
				cityIdIndex: -2,
				title: '',
				canClick: true,
				tip: '加载中...',
				showHeight: 0,
				menuText: '',
				cityText: '',
				//scrollTop:0,
				showNav: true,
				startY: 0, //滑动开始y轴位置
				lastY: 0, //滑动开始y轴位置
				showSearch: true, //是否显示搜索框
				sortIndex:1,
				sortShow:false,
				windowHeight:0
			}
		},

		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var res = uni.getSystemInfoSync();
			self.windowHeight = res.windowHeight;
			console.log('self.windowHeight',self.windowHeight)
			self.$Utils.loadAll(['getMainData', 'getMenuData', 'getCityData'], self);
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		

		/* onPageScroll: function(ev) {
			const self = this;
			if (ev.scrollTop <= 0) {
			  ev.scrollTop = 0;
			}
			//判断浏览器滚动条上下滚动
			if (ev.scrollTop > self.scrollTop || ev.scrollTop == wx.getSystemInfoSync().windowHeight) {
				//向下滚动
				self.showNav = false
				console.log('Xia')
			} else {
				//向上滚动
				self.showNav = true
				console.log('上')
			}
			console.log(ev.scrollTop, self.scrollTop)
			setTimeout(function() {
				self.scrollTop = ev.scrollTop
			}, 100)
		}, */
		onReady() {
			const self = this;
			setTimeout(() => {
				let query = wx.createSelectorQuery();
				query.select('.headBox').boundingClientRect(rect => {
					this.showHeight = rect.height
				}, this).exec();
			}, 300, this)
		},
		onPullDownRefresh() {
			const self = this;
			self.navCurr = 1;
			delete self.searchItem.behavior;
			delete self.searchItem.menu_id;
			delete self.searchItem.location;
			self.menuShow = false;
			self.cityShow = false;
			self.getMainData(true)
		},

		methods: {

			changeSortIndex(e){
				const self = this;
				if(e==-1){
					delete self.searchItem.behavior
				}else{
					 self.searchItem.behavior = e
				};
				self.sortShow = false;
				self.getMainData(true)
			},
			
			shabi(){
				const self = this;
				if(self.title==''){
					delete self.searchItem.title;
					delete self.searchItem.behavior;
					delete self.searchItem.menu_id;
					delete self.searchItem.location;
					self.menuText = '';
					self.cityText = '';
					self.getMainData(true)
				}
			},
			
			search() {
				const self = this;
				if (self.title != '') {
					self.searchItem.title = ['LIKE', ['%' + self.title + '%']],
						self.getMainData(true)
				} else {
					delete self.searchItem.title
					self.getMainData(true)
				}
			},

			changeMenuIndex(index) {
				const self = this;
				self.menuIndex = index;
				if (self.menuIndex == -1) {
					
					delete self.searchItem.menu_id;
					self.menuText = '';
					self.closeMask();
					self.getMainData(true)
				}
			},

			chooseMenuId(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.menuIdIndex = index;
				if (index < 0) {
					if(self.menuIndex == -1){
						delete self.searchItem.menu_id;
						self.menuText = ''
					}else{
						var idArray = [];
						if (self.menuData[self.menuIndex].children.length > 0) {
							for (var i = 0; i < self.menuData[self.menuIndex].children.length; i++) {
								idArray.push(self.menuData[self.menuIndex].children[i].id)
							};
							self.searchItem.menu_id = ['in', idArray];
						
						} else {
							self.searchItem.menu_id = [];
						}
						self.menuText = self.menuData[self.menuIndex].title
					};
					self.menuIdIndex = -2;	
				} else {
					self.searchItem.menu_id = self.menuData[self.menuIndex].children[self.menuIdIndex].id;
					self.menuText = self.menuData[self.menuIndex].children[self.menuIdIndex].title
				};
				self.getMainData(true)
			},



			changeCityIndex(index) {
				const self = this;
				self.cityIndex = index
				if (self.cityIndex == -1) {
					delete self.searchItem.location;
					self.cityText ='';
					self.closeMask();
					self.getMainData(true)
				}
			},

			chooseCityId(index) {
				const self = this;
				uni.setStorageSync('canClick', false);

				self.cityIdIndex = index;
				if (index < 0) {
					if(self.cityIndex==-1){
						delete self.searchItem.location
						self.cityText = '';
					}else{
						var idArray = [];
						if (self.cityData[self.cityIndex].children) {
							for (var i = 0; i < self.cityData[self.cityIndex].children.length; i++) {
								idArray.push(self.cityData[self.cityIndex].children[i].id)
							};
						}
						
						self.searchItem.location = ['in', idArray];
						self.cityText = self.cityData[self.cityIndex].title
					}
					
					self.cityIdIndex = -2;
				} else {
					self.searchItem.location = self.cityData[self.cityIndex].children[self.cityIdIndex].id;
					self.cityText = self.cityData[self.cityIndex].children[self.cityIdIndex].title;
				};
				
				self.getMainData(true)
			},


			getMenuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					type: 3
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

			closeMask() {
				const self = this;
				self.menuShow = false;
				self.cityShow = false;
				self.sortShow = false
			},

			changeCurr(type) {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.navCurr = type;
				if (type == 1) {
					self.canClick = false;
					/* delete self.searchItem.behavior;
					delete self.searchItem.menu_id;
					delete self.searchItem.location;
					self.cityIndex = -1;
					self.cityIdIndex = -2;
					self.menuIndex = -1;
					self.menuIdIndex = -2;
					self.menuText = '';
					self.cityText = '';
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true) */
					
					self.menuShow = false;
					self.cityShow = false;
					self.sortShow = true
				} else if (type == 2) {
					/* self.canClick = false;
					self.searchItem.behavior = 1
					delete self.searchItem.menu_id;
					delete self.searchItem.location;
					self.cityIndex = -1;
					self.cityIdIndex = -2;
					self.menuIndex = -1;
					self.menuIdIndex = -2;
					self.menuText = '';
					self.cityText = '';
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true) */
				} else if (type == 3) {
					/* self.canClick = false;
					self.searchItem.behavior = 2
					delete self.searchItem.menu_id;
					delete self.searchItem.location;
					self.cityIndex = -1;
					self.cityIdIndex = -2;
					self.menuIndex = -1;
					self.menuIdIndex = -2;
					self.menuText = '';
					self.cityText = '';
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true) */
				} else if (type == 4) {
					self.cityShow = false;
					self.menuShow = true;
					self.sortShow = false
				} else if (type == 5) {
					self.menuShow = false;
					self.cityShow = true;
					self.sortShow = false
				}
			},


			getMainData(isNew) {
				const self = this;
				self.tip = '加载中...'
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
				postData.order = {
					listorder: 'desc',
					update_time: 'desc'
				};
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
						for (var i = 0; i < res.info.data.length; i++) {
							res.info.data[i].keywords = res.info.data[i].keywords.split(',')
							console.log('res.info.data[i].keywords',res.info.data[i].keywords)
						}
					};
					self.mainData.push.apply(self.mainData, res.info.data);
					self.canClick = true;
					uni.stopPullDownRefresh();
					uni.setStorageSync('canClick', true);
					self.total = res.info.total;
					if (self.total > self.mainData.length) {
						self.tip = '下拉加载更多'
					} else {
						self.tip = '没有更多内容了'
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
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
							path: '/pages/secondHand-publish/secondHand-publish'
						}
					})
				}
			},


		}
	}
</script>

<style>
	page {
		background-color: #f5f5f5;
		height: 100%;
	}

	.none {
		height: 100%;
		overflow: hidden;
	}

	.headBox {
		position: sticky;
		top: 0;
		z-index: 1000;
	}

	.head {
		line-height: 74rpx;
	}

	.add {
		width: 23rpx;
		height: 23rpx;
		margin-right: 10rpx;
	}


	.ss input {
		border-right: 1px solid #e1e1e1;
		flex: 1;
		padding-left: 20rpx;
		font-size: 28rpx;
		text-align: left;
		box-sizing: border-box;
		color: #222;
	}

	.nav {
		box-shadow: 0 1px 1px 0px rgba(225, 225, 225, 0.7);
	}

	.nav image {
		width: 18rpx;
		height: 9rpx;
		margin-left: 8rpx;
	}

	.nav .item {
		width: 33.3%;
		line-height: 90rpx;
		text-align: center;
	}

	.nav .on {
		position: relative;
		color: #51A9E9;
	}

	/* .nav .on::before {
		content: '';
		width: 100%;
		height: 4rpx;
		background-color: #51A9E9;
		position: absolute;
		bottom: 0;
		left: 0;
	} */

	.list .item image {
		width: 180rpx;
		height: 175rpx;
	}
	
	.list .itemCon {
		height: 180rpx;
	}

	.list .itemCon .tit {
		width: 480rpx
	}

	.list .itemConL {
		width: 180rpx;
		height: 180rpx;
		line-height: 180rpx;
		background-color: #9DD6FF;
	}

	.icon4 {
		width: 14rpx;
		height: 26rpx;
		margin-right: 10rpx;
	}

	.yesBtn {
		padding: 15rpx 30rpx;
	}

	.classfiy .left {
		width: 190rpx;
		background-color: #f5f5f5;
	}

	.classfiy .left .on {
		color: #55AAE9;
		position: relative;
		background: #fff;
	}
	
	.classfiy .sort .on {
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
</style>
