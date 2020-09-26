<template>
	<view :class="menuShow||cityShow?'none':''">
		<view class="head px-3 pb-1 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar +'px'}"
		@click="checkLogin">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		
		<!-- 搜索 -->
		<view class="border-e1 rounded bg-white font-28 mx-3 py-2 d-flex a-center j-sb z-index1000 ss mt-2">
			<input type="text" placeholder="输入关键词搜索" v-model="title"/>
			<view class="px-2" @click="search">搜索</view>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 z-index1000 nav">
			<view class="item" :class="navCurr==1?'on':''" @click="canClick?changeCurr(1):''">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="canClick?changeCurr(2):''">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="canClick?changeCurr(3):''">求购</view>
			<view class="item d-flex a-center j-center" :class="navCurr==4?'on':''" @click="changeCurr(4)">分类
				<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==4"></image>
				<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
			</view>
			<view class="item d-flex a-center j-center" :class="navCurr==5?'on':''" @click="changeCurr(5)">所在地
				<image src="../../static/images/labor-releasel-icon1.png" v-if="navCurr==5"></image>
				<image src="../../static/images/labor-releasel-icon2.png" v-else></image>
			</view>
		</view>
		
		<!-- 列表 -->
		<view class="list">
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" v-for="(item,index) of mainData"
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
		
		
		<view class="oh bg-mask position-fixed d-flex flex-column"  :style="{marginTop:statusBar+155 +'px'}" v-show="menuShow||cityShow">
			<!-- 分类 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="menuShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of menuData" :key="item.id"
					 @click="changeMenuIndex(index)" :class="menuIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white" @click="closeMask">
					<view class="li" :class="menuIdIndex==index?'on':''" @click="chooseMenuId(index)" v-for="(item,index) of menuData[menuIndex].children" 
					:key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="menuIdIndex==index"></image>
					</view>
				</view>
			</view>
			
			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="cityShow">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id"
					 @click="changeCityIndex(index)" :class="cityIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white" @click="closeMask">
					<view class="li" :class="cityIdIndex==index?'on':''" @click="chooseCityId(index)" 
					v-for="(item,index) of cityData[cityIndex].children"
					:key="item.id">{{item.title}}
						<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="cityIdIndex==index"></image>
					</view>
				</view>
			</view>
			
			<view class="flex-1" @click="closeMask"></view>
			
		</view>
		
		
		
		<view class="py-5 font-26 color9 text-center">没有更多内容了</view>
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
				Router:this.$Router,
				statusBar: app.globalData.statusBar,
				navCurr:1,
				classCurr:1,
				addCurr:1,
				ohCurr:0,
				mainData:[],
				now:0,
				Utils:this.$Utils,
				searchItem:{
					thirdapp_id: 2,
					type: 1,
					user_type:0
				},
				menuShow:false,
				cityShow:false,
				menuData:[],
				cityData:[],
				menuIndex:0,
				menuIdIndex:-1,
				cityIndex:0,
				cityIdIndex:-1,
				title:'',
				canClick:true
			}
		},
		
		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getMenuData','getCityData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			const self=  this;
			self.navCurr = 1;
			delete self.searchItem.behavior;
			delete self.searchItem.menu_id;
			delete self.searchItem.location;
			self.menuShow = false;
			self.cityShow = false;
			self.getMainData(true)
		},
		
		methods: {
			
			search(){
				const self = this;
				if(self.title!=''){
					self.searchItem.title = ['LIKE',['%'+self.title+'%']],
					self.getMainData(true)
				}else{
					self.$Utils.showToast('请输入关键词', 'none')
				}
			},
			
			changeMenuIndex(index){
				const self = this;
				self.menuIndex = index
			},
			
			chooseMenuId(index){
				const self = this;
				uni.setStorageSync('canClick', false);
				self.menuIdIndex = index;
				delete self.searchItem.behavior;
				delete self.searchItem.menu_id;
				self.searchItem.menu_id = self.menuData[self.menuIndex].children[self.menuIdIndex].id;
				self.navCurr = 4;
				self.getMainData(true)
			},
			
		
			
			changeCityIndex(index){
				const self = this;
				self.cityIndex = index
			},
			
			chooseCityId(index){
				const self = this;
				uni.setStorageSync('canClick', false);
				self.cityIdIndex = index;
				delete self.searchItem.behavior;
				delete self.searchItem.menu_id;
				self.searchItem.location = self.cityData[self.cityIndex].children[self.cityIdIndex].id;
				self.navCurr = 5;
				self.getMainData(true)
			},
			
			
			getMenuData() {
				const self = this;		
				const postData = {};
				postData.searchItem = {
					type:3
				};
				postData.order = {
					listorder:'desc'
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
					type:2
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.cityData = res.info.data;
					}
					self.$Utils.finishFunc('getCityData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			closeMask(){
				const self = this;
				self.menuShow = false;
				self.cityShow = false;
			},
			
			changeCurr(type){
				const self = this;
				
				uni.setStorageSync('canClick', false);
				if(type==1){
					self.canClick = false;
					self.navCurr = type;
					delete self.searchItem.behavior;
					delete self.searchItem.menu_id;
					delete self.searchItem.location;
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true)
				}else if(type==2){
					self.canClick = false;
					self.navCurr = type;
					self.searchItem.behavior = 1
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true)
				}else if(type==3){
					self.canClick = false;
					self.navCurr = type;
					self.searchItem.behavior = 2
					self.menuShow = false;
					self.cityShow = false;
					self.getMainData(true)
				}else if(type==4){
					self.cityShow = false;
					self.menuShow = true
				}else if(type==5){
					self.menuShow = false;
					self.cityShow = true
				}
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
				postData.order  = {
					listorder:'desc',
					update_time:'desc'
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
				}
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.canClick = true;
					uni.stopPullDownRefresh();
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			checkLogin(){
				const self = this;
				if(!uni.getStorageSync('user_info')||uni.getStorageSync('user_info').headImgUrl==''){
					uni.showModal({
						title:'提示',
						content:'您未登录，是否立即登录',
						success(res) {
							if(res.confirm){
								self.Router.redirectTo({route:{path:'/pages/user/user'}})
							}
						}
					})
				}else{
					self.Router.navigateTo({route:{path:'/pages/secondHand-publish/secondHand-publish'}})
				}
			},
			
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;height: 100%;}
.none{height: 100%;overflow: hidden;}
.head{position: sticky;top: 0;left: 0;right: 0;line-height: 80rpx;}
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}


.ss input{border-right: 1px solid #e1e1e1;flex: 1;padding-left: 20rpx;font-size: 28rpx;text-align: left;box-sizing: border-box;}
.ss input::-webkit-input-placeholder{color: #222!important;}

.nav image{width: 18rpx;height: 9rpx;margin-left: 8rpx;}
.nav .item{width: 20%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.list .item image{width: 180rpx;height: 180rpx;}
.list .itemCon .tit{width: 480rpx;line-height: 1.2;}
.list .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}
</style>
