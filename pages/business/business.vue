<template>
	<view class="h-100 d-flex flex-column">
		<view class="head p-3 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar+23 +'px'}" 
		@click="checkLogin">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		 <!--  --> <!-- :style="{marginTop:statusBar+64 +'px'}" -->
		<view class="d-flex h-100 con flex-1">
			<!-- 左边 -->
			<view class="list font-28 color2 overflow-a">
				<view class="d-flex a-center j-center" :class="listCurr==-2?'on':''" 
				@click="changeList(-2)"><image src="../../static/images/businessl-icon.png" class="icon2"></image>搜索</view>
				<view :class="listCurr==-1?'on':''" @click="changeList(-1)">为您推荐</view>
				<view v-for="(item,index) of menuData" :key="item.id" :class="listCurr==index?'on':''" @click="changeList(index)">{{item.title}}</view>
			</view>
			<!-- 右边 -->
			<view class="flex-1 bg-white h-100 overflow-a" v-show="listCurr==-1">
				<view class="rounded shadow m-2 px-2" v-for="(item,index) of mainData" :key="item.id">
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
						<view class="color6">销售区域：</view>
						<view class="color2 flex-1" :key="c_item.id" v-for="(c_item,c_index) in item.relation">{{c_item.relation_two}}</view>
					</view>
					<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
						<view class="btn Mborder rounded" @click="collect(index)">
							{{item.log&&item.log.length>0&&item.log[0].status==1?'已收藏':'收藏'}}
						</view>
						<view class="btn Mborder rounded ml-5" @click="callPhone(index)">拨打电话</view>
					</view>
					<view class="font-22 color2 py-3"  @click="Router.navigateTo({route:{path:'/pages/user-opinion/user-opinion'}})">不实信息投诉</view>
				</view>
			</view>
			
			<!-- 搜索 -->
			<view class="bg-white flex-1 p-2 search" v-show="listCurr==-2">
				<view class="font-28 color2 d-flex border-e1 a-center px-2 ss">
					<input type="text" value="隧道" />
					<view class="pl-2 borderL-f5">搜索</view>
				</view>
				
				<view class="font-22 color9 d-flex j-sb flex-wrap">
					<view class="w3 py-2">下松挖井</view>
					<view class="w3 py-2">木林森分机</view>
					<view class="w3 py-2">柳工装载机</view>
					<view class="w3 py-2">防水板</view>
				</view>
				
				<!-- 搜索结果 -->
				<view>
					<view class="rounded shadow m-2 px-2" v-for="(item,index) of mainData" :key="item.id">
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
							<view class="color6">销售区域：</view>
							<view class="color2 flex-1" :key="c_item.id" v-for="(c_item,c_index) in item.relation">{{c_item.relation_two}}</view>
						</view>
						<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
							<view class="btn Mborder rounded" @click="collect(index)">
								{{item.log&&item.log.length>0&&item.log[0].status==1?'已收藏':'收藏'}}
							</view>
							<view class="btn Mborder rounded ml-5" @click="callPhone(index)">拨打电话</view>
						</view>
						<view class="font-22 color2 py-3"  @click="Router.navigateTo({route:{path:'/pages/user-opinion/user-opinion'}})">不实信息投诉</view>
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
				Router:this.$Router,
				statusBar: app.globalData.statusBar,
				listCurr:-1,
				menuData:[],
				cityData:[],
				mainData:[],
				searchItem:{
					thirdapp_id: 2,
					type:2,
					user_type:0
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getMenuData','getCityData'], self);
		},
		
		methods: {
			
			callPhone(index){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.mainData[index].phone
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
				if(self.listCurr==-1){
					postData.searchItem.top = 1
				}else if(self.listCurr!=-2){
					postData.getBefore = {
						relation:{
							tableName:'Relation',
							middleKey:'id',
							key:'relation_two_table',
							searchItem:{
								type:['in',[2]]
							},
							condition:'in'
						}
					}
				};
				postData.order  = {
					listorder:'desc',
					update_time:'desc'
				};
				postData.getAfter = {
					relation:{
						tableName:'Relation',
						middleKey:'id',
						key:'relation_one',
						searchItem:{
							status:1,
							type:1
						},
						condition:'=',
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
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			changeList(i){
				const self = this;
				self.mainData = [];
				self.listCurr = i;
				if(self.listCurr!=-2){
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
					behavior:2
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
					self.Router.navigateTo({route:{path:'/pages/business-publish/business-publish'}})
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
			
			getMenuData() {
				const self = this;		
				const postData = {};
				postData.searchItem = {
					type:4
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
		}
	}
</script>

<style>
page{height: 100%;}
.bg-f5{background-color: #f5f5f5;}
/* .head{position: fixed;top: 0;left: 0;right: 0;} */
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}
.icon2{width: 24rpx;height: 24rpx;margin-right: 10rpx;}
.icon3{width: 21rpx;height: 25rpx;}
.img{width: 106rpx;height: 106rpx;margin-top: 20rpx;margin-right: 20rpx;}
.imgBox .img:nth-child(3n){margin-right: 0;}

/* .listBox{width: 180rpx;} */
.list{background-color: #D8EDFE;width: 180rpx;line-height: 100rpx;text-align: center;top: 0;height: 100%;}
.list view{width: 100%;}
.list .on{background-color: #fff;}

.btn{width: 120rpx;line-height: 40rpx;}

.shadow{box-shadow: 0 0rpx 16rpx rgba(114, 130, 138, 0.2)!important;}

.con{overflow-y: scroll;}

.search input{text-align: left;color: #222;font-size: 28rpx;flex: 1;}
.search .ss{height: 80rpx;}
.w3{width: 33.33%;}
.overflow-a{overflow-y: auto;}

.footer{position: static;}
</style>
