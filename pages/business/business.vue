<template>
	<view class="h-100 d-flex flex-column">
		<view class="head px-3 pb-1 bg-mcolor font-30 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar +'px'}"
		 @click="checkLogin">
			<image src="../../static/images/businessl-icon1.png" class="add"></image>
			<view>我的名片</view>
		</view>
		<!--  -->
		<!-- :style="{marginTop:statusBar+64 +'px'}" -->
		<view class="d-flex h-100 con">
			<!-- 左边 -->
			<scroll-view scroll-y="true"  class="list font-28 color2">
				<view style="width: 190rpx;" class="d-flex a-center j-center" :class="listCurr==-2?'on':''" @click="changeList(-2)">
					<image src="../../static/images/businessl-icon.png" class="icon2"></image>搜索
				</view>
				<view style="width: 190rpx;"  :class="listCurr==-1?'on':''" @click="changeList(-1)">为您推荐</view>
				<view style="width: 190rpx;"  v-for="(item,index) of menuData" :key="item.id" :class="listCurr==index?'on':''" @click="changeList(index)">{{item.title}}</view>
			</scroll-view>
			<!-- 右边 -->
			<scroll-view scroll-y="true" @scrolltolower="Bottom()" class=" bg-white h-100">
				<view v-show="listCurr==-1&&noticeData.content" style="background-color: rgb(248,244,212);border: 1px solid rgb(255,221,87);" class="m-2 p-2 mb-0 font-26 con">
					<view class="" style="padding:0;color: rgb(238,51,51);font-size:26rpx" v-html="noticeData.content">
					</view>
				</view>
				<view class="search p-2" v-show="listCurr==-2">
					<view class="font-28 color2 d-flex border-e1 a-center px-2 ss">
						<input type="text" placeholder="请输入关键词" v-model="keywords" />
						<view class="pl-2 borderL-f5 Mcolor" @click="search">搜索</view>
					</view>
					
					<view class="font-22 color9 d-flex flex-wrap" style="height: 80rpx;overflow: hidden;">
						<view class="py-2 mr-1" v-for="(item,index) of historyData" :key="item.id" @click="clickSearch(index)">{{item.result}}</view>
					</view>
				</view>
				<view class="font-28 color2 d-flex a-center j-sb bg-fc  z-index1000 nav" style="position: sticky;top: 0;"  v-show="listCurr!=-2&&listCurr!=-1">
					<view class="item d-flex a-center j-center" :class="menuBefore.relation1?'on':''" @click="changeCurr(1)">
					{{menuText!=''?menuText:'分类'}}
						<image src="../../static/images/labor-releasel-icon1.png" v-if="menuBefore.relation1"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view>
					<!-- <view class="item d-flex a-center j-center" :class="navCurr==2?'on':''" @click="changeCurr(2)">销售区域
						<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==2"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view> -->
					<view class="item d-flex a-center j-center" :class="searchItem.location?'on':''" @click="changeCurr(3)">
						{{cityText!=''?cityText:'所在地'}}
						<!-- {{cityIndex>=0&&cityIdIndex<0?cityData[cityIndex].title:(cityIndex>=0&&cityIdIndex>=0?cityData[cityIndex].children[cityIdIndex].title:'所在地')}} -->
						<image src="../../static/images/labor-releasel-icon1.png" v-if="searchItem.location"></image>
						<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
					</view>
				</view>
				<view class="rounded shadow m-2 px-2 bg-fc pb-3 border-f5" v-for="(item,index) of mainData" :key="item.id">
					<view class="Mcolor font-30 py-3">{{item.title}}</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-de1">
						<view class="d-flex a-center"><text class="pr-2">{{item.name}}</text><view class="mr-2" style="width: 1px;height: 28rpx;background-color: #e1e1e1;"></view>{{item.phone}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
							<view class="font-24 color6 pl-1">{{item.city?item.city.title:''}}</view>
						</view>
					</view>
					<view class="d-flex font-26 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">
							<view class="w-100 overflow-h" style="word-break:break-all;">{{item.description?item.description.substr(0,100):''}}</view>
							<view class="d-flex font-24">
								<view class="d-flex a-center flex-wrap" v-if="item.mainImg.length>0">
									<image mode="aspectFill" @click="imgPreview(index,c_index)" v-for="(c_item,c_index) of item.mainImg" :key="c_index" :src="c_item.url" class="img"></image>
								</view>
							</view>
						</view>
						
					</view>
					
					
					<!-- <view class="d-flex font-24 pt-3">
						<view class="color6">销售区域：</view>
						<view class="color2 mr-1" :key="c_item.id" v-for="(c_item,c_index) in item.relation">{{c_item.relation_two}}</view>
					</view> -->
					<view class="font-22 Mcolor text-center d-flex a-center j-sb pt-3">
						<view class="d-flex a-center" @click="collect(index)">
							<image :src="item.log&&item.log.length>0&&item.log[0].status==1?'../../static/images/detailsl-icon1.png':'../../static/images/detailsl-icon5.png'" 
							class="icon6"></image>
							<view class="pl-1 color6">{{item.log&&item.log.length>0&&item.log[0].status==1?'已收藏':'收藏'}}</view>
						</view>
						
						<view class="btn Mborder rounded ml-5" @click="callPhone(index)">拨打电话</view>
					</view>
				</view>
				<view class="py-5 font-26 color9 text-center">{{tip}}</view>
			</scroll-view>


		</view>

		<view class="oh bg-mask position-fixed top-0  right-0"  style="left:190rpx;bottom: 120rpx;" @click="closeMask" :style="{marginTop:statusBar+87 +'px'}"
		 v-show="menuShow||cityShow||saleCityShow">
			<!-- 分类 -->

		</view>
		<scroll-view scroll-y   :style="'margin-top:'+(statusBar+87)+'px'" style="z-index:41;left:190rpx;overflow-y: auto;" 
		class="position-fixed top-0  right-0">
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="menuShow">
				<!-- <view class="left">
					<view class="li py-3" v-for="(item,index) of menuData" :key="item.id" @click="changeMenuIndex(index)" :class="menuIndex==index?'on':''">{{item.title}}</view>
				</view> -->
				<scroll-view class="right flex-1 bg-white" :style="'height:'+(windowHeight*0.8-217)+'px'" scroll-y="true">
					<view class="li" :class="menuIdIndex==-1?'on':''" @click="chooseMenuId(-1)">全部
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="menuIdIndex==-1"></image>
					</view>
					<view class="li" :class="menuIdIndex==index?'on':''" @click="chooseMenuId(index)" v-for="(item,index) of menuData[menuIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="menuIdIndex==index"></image>
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

			<!-- <view class="classfiy font-26 color2 line-h text-center d-flex" v-show="saleCityShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id" @click="changeSaleCityIndex(index)" :class="citySaleIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li" :class="citySaleIdIndex==index?'on':''" @click="chooseSaleCityId(index)" v-for="(item,index) of cityData[citySaleIndex].children"
					 :key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="citySaleIdIndex==index"></image>
					</view>
				</view>
			</view> -->
		</scroll-view>



		<view style="height: 120rpx;width: 100%;"></view>
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
					user_type: ['in',[0,2]]
				},
				historyData: [],
				keywords: '',
				cityShow: false,
				menuShow: false,
				saleCityShow: false,
				cityIndex: -1,
				cityIdIndex: -2,
				menuIndex: 0,
				menuIdIndex: -1,
				navCurr: -1,
				
				citySaleIndex: 0,
				citySaleIdIndex: -1,
				saleCityBefore: {},
				menuBefore: {},
				windowHeight:0,
				tip: '加载中...',
				menuText: '',
				cityText: '',
				noticeData:{}
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var res = uni.getSystemInfoSync();
			self.windowHeight = res.windowHeight;
			self.$Utils.loadAll(['getNoticeData','getMainData', 'getMenuData', 'getCityData', 'getHistoryData'], self);
		},

		methods: {
			
			getNoticeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					title:['in','公告']
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.noticeData = res.info.data[0];
					};
					self.$Utils.finishFunc('getNoticeData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			imgPreview(index,c_index){
				const self = this;
				var urls = [];
				for (var i = 0; i < self.mainData[index].mainImg.length; i++) {
					urls.push(self.mainData[index].mainImg[i].url)
				};
				uni.previewImage({
					current:c_index,
					urls:urls,
				})
			},
			
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
				if(index<0){
					self.menuBefore = {};
					self.menuText = '';
				}else{
					
					//self.listCurr = self.menuIndex;
					/* self.menuBefore = {
						tableName: 'Relation',
						middleKey: 'id',
						key: 'relation_one',
						searchItem: {
							
							relation_two: ['in', [self.menuData[self.menuIndex].children[self.menuIdIndex].id]]
						},
						condition: 'in'
					}; */
					self.menuBefore = {
						relation1: {
							tableName: 'Relation',
							middleKey: 'id',
							key: 'relation_one',
							searchItem: {
								relation_two: ['in', [self.menuData[self.menuIndex].children[self.menuIdIndex].id]]
							},
							condition: 'in'
						}
					}
					self.menuText = self.menuData[self.menuIndex].children[self.menuIdIndex].title
				};
				/* self.cityIndex = -1;
				self.cityIdIndex = -1;
				self.saleCityBefore = {};
				delete self.searchItem.location; */
				self.navCurr = 1;
				self.menuShow = false;
				self.getMainData(true)
			},
			
			changeCityIndex(index) {
				const self = this;
				self.cityIndex = index
				if (self.cityIndex == -1) {
					delete self.searchItem.location;
					self.cityText ='';
					self.closeMask();
					
				}else{
					self.cityIdIndex = -2;
					var idArray = [];
					if (self.cityData[self.cityIndex].children) {
						for (var i = 0; i < self.cityData[self.cityIndex].children.length; i++) {
							idArray.push(self.cityData[self.cityIndex].children[i].id)
						};
					}
					
					self.searchItem.location = ['in', idArray];
					self.cityText = self.cityData[self.cityIndex].title
				}
				self.getMainData(true)
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
			

			

			closeMask() {
				const self = this;
				self.menuShow = false;
				self.cityShow = false;
				self.saleCityShow = false
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
						self.historyData.unshift({
							result: self.keywords,
							id: res.info.id
						});
						console.log('self.historyData',self.historyData)
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
				//postData.getBefore = {};
				console.log('self.listCurr',self.listCurr)
				if (self.listCurr == -1) {
					self.keywords = '';
					postData.searchItem.top = 1;
					delete self.searchItem.location;
					self.menuBefore = {};
					self.keywords = '';
					postData.order = {
						listorder: 'desc'
					};
				} else if (self.listCurr != -2&&JSON.stringify(self.menuBefore)=="{}") {
					console.log(13344)
					self.keywords = '';
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
				console.log('246567',postData.getBefore)
				if (JSON.stringify(self.menuBefore) != '{}'&&self.listCurr!=-1) {
					//postData.getBefore.relation1 = self.menuBefore
					postData.getBefore = self.menuBefore
				};
				/* if (JSON.stringify(self.saleCityBefore) != '{}'&&self.listCurr!=-1) {
					postData.getBefore.relation2 = self.saleCityBefore
				}; */
				if (self.keywords != '') {
					postData.keywords = self.keywords
				};
				postData.getAfter = {
					/* relation: {
						tableName: 'Relation',
						middleKey: 'id',
						key: 'relation_one',
						searchItem: {
							status: 1,
							type: 1
						},
						condition: '=',
					}, */
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
					self.total = res.info.total;
					if (self.total > self.mainData.length) {
						self.tip = '下拉加载更多'
					} else {
						self.tip = '没有更多内容了'
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.searchCard(postData, callback);
			},

			changeList(i) {
				const self = this;
				self.mainData = [];
				self.listCurr = i;
			
				if(self.listCurr>=0){
					delete self.searchItem.location
					self.cityText = ''
					self.menuIndex = self.listCurr;
					self.menuIdIndex = -1;
					self.menuText = '';
					self.menuBefore = {};
				}else{
					self.menuShow = false
					self.saleCityShow = false;
					self.cityShow = false;
					if(self.listCurr==-1){
						delete self.searchItem.location
						self.cityText = ''
					}
				}
				if (self.listCurr != -2) {
					self.getMainData(true)
				};
			},


			collect(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.mainData[index].log.length == 0) {
					self.addGoodLog(index)
				} else {
					self.updateGoodLog(index)
				};
				console.log(235)
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


			updateGoodLog(index) {
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
							path: '/pages/business-publish/business-publish?type=isMe'
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
	.icon6{width: 32rpx;height: 32rpx;}
	.head{line-height: 37px;}
	.add {
		width: 23rpx;
		height: 23rpx;
		margin-right: 10rpx;
	}

	.icon2 {
		width: 24rpx!important;
		height: 24rpx!important;
		margin-right: 10rpx;
	}

	.icon3 {
		width: 21rpx!important;
		height: 25rpx!important;
	}

	.img {
		width: 106rpx;
		height: 106rpx;
		margin-top: 20rpx;
		margin-right: 9rpx;
	}

	.imgBox .img:nth-child(3n) {
		margin-right: 0;
	}

	/* .listBox{width: 180rpx;} */
	.list {
		background-color: #D8EDFE;
		width: 190rpx;
		line-height: 90rpx;
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
		
		box-shadow: 2rpx 3rpx 3rpx 0rpx 
				rgba(0, 0, 0, 0.16)!important;
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

	/* .footer {
		position: static;
	} */
	.nav{box-shadow: 0 1px 1px 0px rgba(225, 225, 225, 1)}
	.nav image {
		width: 18rpx;
		height: 9rpx;
		margin-left: 8rpx;
	}

	.nav .item {
		width: 50%;
		line-height: 45px;
		text-align: center;
	}

	.nav .on {
		position: relative;
		color: #51A9E9;
		/* box-shadow: 0 8px 6px -6px rgba(148, 232, 241, 0.5); */
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
