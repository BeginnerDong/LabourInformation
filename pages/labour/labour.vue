<template>
	<view>
		<view class="head px-3 py-2 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar+23 +'px'}" 
		@click="checkLogin">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		
		<!-- 搜索 -->
		<view class="bg-f5 px-3 pt-2">
			<view class="border-e1 rounded font-28 px-3 py-2 d-flex a-center j-sb ss">
				<input type="text" v-model="keywords"/>
				<view class="px-2" @click="search">搜索</view>
			</view>
		</view>
		
		<view class="iconBox d-flex a-center j-sb px-3 pt-4 bg-f5">
			<image @click="changeBehavior(1)" :src="navType==1?'../../static/images/laborl-icon.png':'../../static/images/laborl-icon5.png'" mode=""></image>
			<image @click="changeBehavior(2)" :src="navType==2?'../../static/images/laborl-icon9.png':'../../static/images/laborl-icon1.png'" mode=""></image>
			<image @click="changeBehavior(3)" :src="navType==3?'../../static/images/laborl-icon8.png':'../../static/images/laborl-icon2.png'" mode=""></image>
			<image @click="changeBehavior(4)" :src="navType==4?'../../static/images/laborl-icon7.png':'../../static/images/laborl-icon3.png'" mode=""></image>
			<image @click="changeBehavior(5)" :src="navType==5?'../../static/images/laborl-icon6.png':'../../static/images/laborl-icon4.png'" mode=""></image>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">默认</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">介绍费</view>
			<view class="item d-flex a-center j-center" :class="navCurr==3?'on':''" @click="changeNav(3)">
				所在地
				<image src="../../static/images/second-handl-icon4.png" v-if="navCurr==3"></image>
				<image src="../../static/images/second-handl-icon.png" v-else></image>
			</view>
		</view>
		
		<view class="bg-f5">
			<view class="bg-white mb-2 px-3"  v-for="(item,index) of mainData"
			:key="item.id" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/labour-detail/labour-detail?id='+$event.currentTarget.dataset.id}})">
				<view class="font-30 color2 pt-4">{{item.title}}</view>
				<view class="d-flex flex-wrap py-3 imgBox">
					<image v-for="(c_item,c_index) in item.mainImg" :key="c_index"  :src="c_item.url" mode=""></image>
					
				</view>
				<view class="d-flex a-center j-sb pb-3">
					<view class="d-flex a-center">
						<view class="tag tagB" v-if="item.behavior==1">招工人</view>
						<view class="tag tagB" v-if="item.behavior==2">招队伍</view>
						<view class="tag tagG" v-if="item.behavior==3">工人找活</view>
						<view class="tag tagG" v-if="item.behavior==4">队伍找活</view>
						<view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view>
						<view v-if="item.price!=''"><text class="tag tagR">介绍费</text><text class="tag tagO">{{item.price}}</text></view>
					</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon3.png" class="icon2"></image>
						<view class="font-24 color6 pl-1">{{item.city?item.city.title:''}}</view>
						<view class="font-24 color6 pl-2">{{Utils.formatMsgTime(item.update_time)}}</view>
					</view>
				</view>	
				<view class="line-h-md py-4 font-24 color6 dashedBorder">
					<view v-if="item.behavior==1" v-for="(c_item,c_index) in item.passage_array" :key="c_index">{{c_item.name}}，{{c_item.num}}名，
					{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
					<view v-if="item.behavior==2" v-for="(c_item,c_index) in item.passage_array" :key="c_index">{{c_item.name}}，{{c_item.num}}支队伍，
					{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
					<view v-if="item.behavior==3">求职岗位：{{item.description}},期待工资：{{item.salary}}</view>
					<view v-if="item.behavior==4">我能承包：<span :key="c_index" v-for="(c_item,c_index) in item.passage_array">{{c_item.name}}</span></view>
				</view>
			</view>
		
			
			<view class="py-5 font-26 color9 text-center">{{total>mainData.length?'下拉加载更多':'没有更多内容了'}}</view>
		</view>
		
		
		<!-- 未搜索到信息 -->
<!-- 		<view class="my-5 font-26 color2 line-h text-center">
			<view class="pb-3">没有搜索到与“装载机”相关的信息</view>
			<view>请试试其他关键词</view>
		</view> -->
		
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0"  
		@click="closeMask" style="top: 78px;" v-show="cityShow">
			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex">
				<view class="left">
					<view class="li py-3" v-for="(item,index) of cityData" :key="item.id"
					 @click="changeCityIndex(index)" :class="cityIndex==index?'on':''">{{item.title}}</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li" :class="cityIdIndex==index?'on':''" @click="chooseCityId(index)" 
					v-for="(item,index) of cityData[cityIndex].children"
					:key="item.id">{{item.title}}
						<image src="../../static/images/used to releasel-icon5.png" class="icon5" v-if="cityIdIndex==index"></image>
					</view>
				</view>
			</view>
			
		</view>
		
		
		<view style="height: 100rpx;width: 100%;"></view> 
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
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/business/business'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>商务通</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/labour/labour'}})">
				<image src="../../static/images/nabar4-a.png" mode=""></image>
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
				searchItem:{
					thirdapp_id: 2,
					type: 3,
					user_type:0
				},
				mainData:[],
				now:0,
				Utils:this.$Utils,
				total:0,
				navType:1,
				cityShow:false,
				cityIndex:0,
				cityIdIndex:-1,
				cityData:[],
				keywords:''
			}
		},
		
		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getCityData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			search(){
				const self = this;
				uni.setStorageSync('canClick', true);
				if(self.keywords==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入关键词搜索', 'none')
				}else{
					self.getMainData(true)
				}
			},
			
			changeBehavior(type){
				const self = this;
				if(self.navType!=type){
					self.navType=type;
					if(self.navType==1){
						delete self.searchItem.behavior
					}else if(self.navType==2){
						self.searchItem.behavior = 1
					}else if(self.navType==3){
						self.searchItem.behavior = 2
					}else if(self.navType==4){
						self.searchItem.behavior = 3
					}else if(self.navType==5){
						self.searchItem.behavior = 4
					}
					self.getMainData(true)
				}
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
			
			
			
			changeNav(type){
				const self = this;
				uni.setStorageSync('canClick', false);
				if(type==1){
					self.navCurr = type;
					delete self.searchItem.price;
					delete self.searchItem.location;
					self.cityShow = false;
					self.getMainData(true)
				}else if(type==2){
					self.navCurr = type;
					self.searchItem.price = ['not in',[]]
					self.cityShow = false;
					self.getMainData(true)
				}else if(type==3){
					self.cityShow = true
				}
			},
			
			changeCityIndex(index){
				const self = this;
				self.cityIndex = index
			},
			
			chooseCityId(index){
				const self = this;
				uni.setStorageSync('canClick', false);
				self.cityIdIndex = index;
				delete self.searchItem.price;
				self.searchItem.location = self.cityData[self.cityIndex].children[self.cityIdIndex].id;
				self.navCurr = 3;
				self.cityShow = false;
				self.getMainData(true)
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
				/* postData.order  = {
					listorder:'desc',
					update_time:'desc'
				}; */
				if(self.keywords!=''){
					postData.keywords = self.keywords
				};
				postData.getAfter = {
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
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.total = res.info.total;
					
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.searchLabour(postData, callback);
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
					self.Router.navigateTo({route:{path:'/pages/labour-publish/labour-publish'}})
				}
			},
		}
	}
</script>

<style>
.bg-f5{background-color: #f5f5f5;}
.head{position: sticky;top: 0;left: 0;right: 0;}
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}


.ss input{border-right: 1px solid #e1e1e1;flex: 1;text-indent: 20rpx;font-size: 28rpx;text-align: left;}
.ss input::-webkit-input-placeholder{color: #222!important;}

.iconBox image{width: 110rpx;height: 110rpx ;}
.nav image{width: 13rpx;height: 6rpx;margin-left: 8rpx;}
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.imgBox image{width: 160rpx;height: 160rpx;margin-right: 17rpx;}
.imgBox image:nth-child(4n){margin-right: 0;}
.imgBox image:nth-child(n+5){margin-top: 20rpx;}
.icon2{width: 21rpx;height: 25rpx;}

.tagR{margin-right: 0;}

.dashedBorder{border-top: 1px dashed #e1e1e1;}
.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}
</style>
