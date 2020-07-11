<template>
	<view>
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">求购</view>
		</view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-e1">
			<view>选择</view>
			<view class="Mcolor d-flex a-center">
				<view class="pr-5 borderR-f5" @click="showToast()">删除</view>
				<view class="pl-5">一键更新日期</view>
			</view>
		</view>
		
		<view>
			<view class="borderB-e1 py-4">
				<view class="d-flex a-center mx-3">
					<image src="../../static/images/i releasel-icon.png" class="icon1"></image>
					<!-- <image src="../../static/images/i releasel-icon1.png" class="icon1"></image> -->
					<view class="ml-2 d-flex a-center j-sb flex-1">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img"></image>
						<view class="itemCon flex-1 ml-2">
							<view class="color3 font-28 avoidOverflow2 tit">
								{{item.title}}
							</view>
							<view class="font-22 color6 d-flex a-center j-sb mt-2 line-h">
								<view class="Rcolor">{{item.price==''?'面议':item.price}}</view>
								<view>{{item.city?item.city.title:''}}</view>
							</view>
							<view class="font-22 d-flex a-center h-100 mt-2">
								<view class="tag tagB" v-if="item.behavior==1">出售</view>
								<view class="tag tagG" v-if="item.behavior==2">求购</view>
								<view class="tag tagO">{{item.label?item.label.title:''}}</view>
								<view class="tag tagR" v-if="item.top>0">置顶</view>
								<view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view>
							</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 py-3 mx-3 d-flex j-end">
					<view>首发时间：{{item.create_time?item.create_time:''}}</view>
					<view class="pl-2">最近更新时间：{{item.update_time?item.update_time:''}}</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="showToast()">删除</view>
					<view class="borderL-e1 borderR-e1">编辑</view>
					<view>更新时间</view>
				</view>
			</view>
		</view>

		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:1,
				mainData:[],
				now:0,
				Utils:this.$Utils,
				searchItem:{
					thirdapp_id: 2,
					type: 1,
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			changeNav(i){
				const self = this;
				self.navCurr = i
			},
			showToast(){
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定删除该条信息？'
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
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.order  = {
					listorder:'desc',
					update_time:'desc'
				};
				postData.getAfter = {
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
				}

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	}
</script>

<style>
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 40rpx;height: 40rpx;}
.img{width: 160rpx;height: 160rpx;}

.oh view{width: 33.33%;}
</style>
