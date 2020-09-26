<template>
	<view>
		<view class="head px-3 bg-mcolor line-h" :style="{paddingTop:statusBar +'px'}">
			<image src="../../static/images/logo.png" class="logo"></image>
			<!-- <view class="font-36 colorf d-flex a-center py-2">桥隧之家</view> -->
			<!-- <view class="colorf font-24 top overflow-h">欢迎来到桥隧之家，在这里您可以买卖二手设备，劳务招聘，在这里您可以买卖二手设备，劳务招聘</view> -->
			<uni-notice-bar background-color="#51A9E9" color="#fff" single="true" scrollable="true" v-if="text!=''" :text="text"></uni-notice-bar>
		</view>

		<!-- banner -->
		<view class="banner" :style="{marginTop:statusBar*2+130 +'rpx'}">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
				<block v-for="(item,index) in sliderData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" />
					</swiper-item>
				</block>
			</swiper>
		</view>

		<block v-for="(item,index) in mainData" :key="index">
			<!-- 推荐 -->
			
			
			<view class="p-3 mb-2 bg-white indexBox1" v-if="item.style==1">
				<view class="d-flex a-center j-sb" style="width: 100%;" @click="addViewCount(index)">
					<view class="d-flex flex-column j-sb  h-100" style="width: 100%;" :data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="font-32 color2 flex-1 mb-2 avoidOverflow2">{{item.title}}</view>
						<view class="d-flex a-center j-sb h-100">
							<view class="font-22 d-flex a-center">
								<view class="tag tag1" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
								<view class="tag tag2" v-if="item.top==1">推荐</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/home-icon.png" class="icon1"></image>
								<view class="font-24 color6 pl-1 flex-1">{{item.view_count}}</view>
							</view>
						</view>
					</view>
				</view>
				<view class="bg-f5 d-flex a-center p-1 mt-3">
					<image src="../../static/images/home-icon2.png" class="icon2"></image>
					<view class="font-24 color6 pl-1 flex-1">{{item.description}}</view>
				</view>
			</view>
			
			<view class="p-3 mb-2 bg-white indexBox1" v-if="item.style==2">
				<view class="d-flex a-center j-sb" @click="addViewCount(index)">
					<view class="d-flex flex-column j-sb mr-3 h-100" :data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="font-32 color2 flex-1 mb-3 avoidOverflow2 tjTit">{{item.title}}</view>
						<view class="d-flex a-center j-sb h-100 mt-3">
							<view class="font-22 d-flex a-center">
								<view class="tag tag1" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
								<view class="tag tag2" v-if="item.top==1">推荐</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/home-icon.png" class="icon1"></image>
								<view class="font-24 color6 pl-1 flex-1">{{item.view_count}}</view>
							</view>
						</view>
					</view>
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="tjImg"></image>
				</view>
				<view class="bg-f5 d-flex a-center p-1 mt-3">
					<image src="../../static/images/home-icon2.png" class="icon2"></image>
					<view class="font-24 color6 pl-1 flex-1">{{item.description}}</view>
				</view>
			</view>

			<view  @click="addViewCount(index)">
				<view class="mb-2 p-3 bg-white indexBox2" v-if="item.style==3" :data-id = "item.id"
				@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="font-32 color2">{{item.title}}</view>
					<view class="d-flex a-center mt-3 flex-wrap imgBox">
						<image v-for="(c_item,c_index) in item.mainImg" :key="c_index" :src="c_item.url" mode=""></image>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
						</view>
						<view class="d-flex a-center">
							<image src="../../static/images/home-icon.png" class="icon1"></image>
							<view class="font-24 color6 pl-1 flex-1">{{item.view_count}}</view>
						</view>
					</view>
					<view class="bg-f5 d-flex a-center p-1 mt-3">
						<image src="../../static/images/home-icon2.png" class="icon2"></image>
						<view class="font-24 color6 pl-1 flex-1">{{item.description}}</view>
					</view>
				</view>
			</view>
			

			<view class="mb-2 p-3 bg-white indexBox3" v-if="item.style==4"> 
				<view class="font-32 color2">{{item.title}}</view>
				<view class="mt-3">
					<video class="video" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" controls>
						
					</video>
				</view>
				<view class="d-flex a-center j-sb h-100 mt-3">
					<view class="font-24 color6 tag tag1">{{item.keywords&&item.keywords[0]?item.keywords[0]:''}}</view>
					<view class="d-flex a-center">
						<image src="../../static/images/home-icon.png" class="icon1"></image>
						<view class="font-24 color6 pl-1 flex-1">{{item.view_count}}</view>
					</view>
				</view>
				<view class="bg-f5 d-flex a-center p-1 mt-3">
					<image src="../../static/images/home-icon2.png" class="icon2"></image>
					<view class="font-24 color6 pl-1 flex-1">{{item.description}}</view>
				</view>
			</view>
		</block>

		<view class="py-5 font-26 color9 text-center">{{total>mainData.length?'下拉加载更多':'加载中'}}</view>
		<view style="height: 100rpx;width: 100%;"></view>
		<!-- 底部 -->
		<view class="footer">
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
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
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	export default {
		components: {uniNoticeBar},
		data() {
			return {
				Router: this.$Router,
				is_show: false,
				statusBar: app.globalData.statusBar,
				total:0,
				mainData:[],
				sliderData:[],
				text:''
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getNoticeData','getMainData','getSliderData','getUserData'], self);
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
			
			addViewCount(index){
				const self = this;
				console.log(234)
				self.mainData[index].view_count++
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.refreshToken = true;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						uni.setStorageSync('kefu',uni.getStorageSync('user_info').thirdApp.phone)
					}
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			getNoticeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '通知'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.text = res.info.data[0].description
					};
					self.$Utils.finishFunc('getNoticeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '轮播图'
				};
				postData.getAfter = {
					child:{
						tableName:'Label',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'=',
						order:{
							listorder:'desc'
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0&&res.info.data[0].child.length>0) {
						self.sliderData = res.info.data[0].child
					};
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 1
				};
				postData.order  = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].keywords = self.mainData[i].keywords.split(',')
						}
					};
					self.total = res.info.total;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		}
	};
</script>
<style>
	page {
		background-color: #f5f5f5;
	}
	.head{position: fixed;top: 0;left: 0;right: 0;z-index: 1000;}

	.top {
		height: 65rpx;
		line-height: 65rpx;
		white-space: nowrap;
	}

	.banner {
		height: 360rpx;
	}

	.swiper-box {
		width: 100%;
		height: 100%;
	}

	.banner image {
		width: 100%;
		height: 100%;
	}

	/* .icon1 {
		width: 30rpx !important;
		height: 22rpx !important;
	} */

	.tjTit {
		width: 480rpx;
	}

	.tjImg {
		width: 210rpx;
		height: 160rpx;
	}

	/* .icon2 {
		width: 27rpx !important;
		height: 27rpx !important;
	} */

	.imgBox image {
		width: 210rpx;
		height: 160rpx;
		margin-right: 31rpx;
	}

	.imgBox image:nth-child(3n) {
		margin-right: 0;
	}

	.imgBox image:nth-child(n+4) {
		margin-top: 20rpx;
	}

	.video {
		height: 340rpx;
		width: 100%;
	}
	
	.bg-f5{background-color: rgba(245,245,245,0.7);}
</style>
