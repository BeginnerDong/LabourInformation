<template>
	<view>
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb  bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">招工人</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">招队伍</view>
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">工人找活</view>
			<view class="item" :class="navCurr==5?'on':''" @click="changeNav(5)">队伍找活</view>
		</view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-f5">
			<view @click="chooseShow" v-if="!isShowChoose" class="btn2">选择</view>
			<view @click="chooseShow" v-else class="btn2">取消</view>
			<view class="Mcolor d-flex a-center" v-if="isShowChoose">
				<view class="pr-5 borderR-f5" @click="chooseAll()">全选</view>
				<view class="pr-5 borderR-f5" @click="deleteAll()">删除</view>
				<view class="pl-5" @click="updateAll()">一键更新日期</view>
			</view>
		</view>
		
		<view>
			<view class="borderB-f5 py-4" v-for = "(item,index) of mainData" :key="item.id">
				<view class="d-flex a-center ml-3">
					<image @click="choose(index)" v-if="isShowChoose" :src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon1"></image>
					<!-- <image src="../../static/images/i-releasel-icon1.png" class="icon1"></image> -->
					<view class="bg-white px-3 flex-1" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
						<view class="font-30 color2 pt-4">{{item.title}}</view>
						<view class="d-flex flex-wrap pt-3 imgBox">
							<image v-for="(c_item,c_index) in item.mainImg" :key="c_index"  :src="c_item.url" mode=""></image>
						</view>
						<view class="d-flex a-center j-sb py-3">
							<view>
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
							</view>
						</view>
						<view class="line-h-md  font-24 color6 dashedBorder">
							<view v-if="item.behavior==1" v-for="(c_item,c_index) in item.passage_array" :key="index">{{c_item.name}}，{{c_item.num}}名，
							{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
							<view v-if="item.behavior==2" v-for="(c_item,c_index) in item.passage_array" :key="index">{{c_item.name}}，{{c_item.num}}支队伍，
							{{c_item.money!=''?'月工资'+c_item.money:'待遇面议'}}</view>
							<view v-if="item.behavior==3">求职岗位：{{item.description}},期待工资：{{item.salary}}</view>
							<view v-if="item.behavior==4">我能承包：<span :key="index" v-for="(c_item,c_index) in item.passage_array">{{c_item.name}}</span></view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 py-3 mx-3 d-flex j-end">
					<view>首发时间：{{item.create_time?item.create_time:''}}</view>
					<view class="pl-2">最近更新时间：{{item.update_time?item.update_time:''}}</view>
				</view>
				<view class="Mcolor font-30 d-flex j-sb a-center text-center line-h oh">
					<view @click="deleteAll(index)">删除</view>
					<view class="borderL-e1 borderR-e1" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/labour-publish/labour-publish?id='+$event.currentTarget.dataset.id}})">编辑</view>
					<view @click="updateAll(index)">更新时间</view>
				</view>
			</view>
			
		</view>
		
		
		<!-- 删除弹框 -->
		
		
	</view>
</template>

<script>
	import Vue from 'vue'
	export default {
		data() {
			return {
				Router:this.$Router,
				navCurr:1,
				mainData:[],
				now:0,
				Utils:this.$Utils,
				searchItem:{
					thirdapp_id: 2,
					type: 3,
				},
				isShowChoose:false,
				isChooseAll:false,
				allId:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
			//self.checkChooseAll()
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
			
			
			
			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				console.log('self.isChooseAll',self.isChooseAll)
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].choose = self.isChooseAll;
					//self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
					if(self.isChooseAll){
						self.allId.push(self.mainData[i].id)
					}else{
						self.allId = []
					}
				};
				//self.countTotalPrice();
			},
			
			choose(index){
				const self = this;
				self.mainData[index].choose = !self.mainData[index].choose
				Vue.set(self.mainData,index,self.mainData[index])
				var options = self.allId.indexOf(self.mainData[index].id);
				if(options>=0){
					self.allId.splice(1,parseInt(options))
				}else{
					self.allId.push(self.mainData[index].id)
				}
			},
			
			chooseShow(){
				const self = this;
				self.isShowChoose = !self.isShowChoose
			},
			
			changeNav(i){
				const self = this;
				if(i!=self.navCurr){
					self.navCurr = i
					if(self.navCurr==1){
						delete self.searchItem.behavior
					}else if(self.navCurr==2){
						self.searchItem.behavior = 1
					}else if(self.navCurr==3){
						self.searchItem.behavior = 2
					}else if(self.navCurr==4){
						self.searchItem.behavior = 3
					}else if(self.navCurr==5){
						self.searchItem.behavior = 4
					}
					self.getMainData(true)
				}
			},
			
			deleteAll(index) {
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定删除所选信息？',
					success(res) {
						if(res.confirm){
							const postData = {};
							postData.tokenFuncName = 'getProjectToken';
							postData.data = {status:-1};
							postData.searchItem = {
								thirdapp_id:2
							}
							if(index||index==0){
								postData.searchItem.id = self.mainData[index].id
							}else{
								postData.searchItem.id = ['in',self.allId]
							};
							const callback = (data) => {				
								if (data.solely_code == 100000) {					
									self.$Utils.showToast('操作成功', 'none', 1000)
									self.isShowChoose = false;
									setTimeout(function() {
										self.getMainData(true);
										
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(data.msg, 'none', 1000)
								}	
							};
							self.$apis.messageUpdate(postData, callback);
						}
					}
				})
			},
			
			updateAll(index) {
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定更新所选信息？',
					success(res) {
						if(res.confirm){
							const postData = {};
							postData.tokenFuncName = 'getProjectToken';
							postData.data = {status:1};
							postData.searchItem = {
								thirdapp_id:2
							}
							if(index||index==0){
								postData.searchItem.id = self.mainData[index].id
							}else{
								postData.searchItem.id = ['in',self.allId]
							};
							const callback = (data) => {				
								if (data.solely_code == 100000) {					
									self.$Utils.showToast('操作成功', 'none', 1000)
									self.isShowChoose = false;
									setTimeout(function() {
										self.getMainData(true);
										
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(data.msg, 'none', 1000)
								}	
							};
							self.$apis.messageUpdate(postData, callback);
						}
					}
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
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].create_time = self.mainData[i].create_time.substr(0,10);
							self.mainData[i].update_time = self.mainData[i].update_time.substr(0,10);
							self.mainData[i].choose = false
						}
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
.nav .item{padding: 0 30rpx;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 4rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 40rpx;height: 40rpx;}

.oh view{width: 33.33%;}

.imgBox image{width: 145rpx;height: 145rpx;margin-right: 20rpx;}
.imgBox image:nth-child(4n){margin-right: 0;}
.icon2{width: 21rpx;height: 25rpx;}
.tagR{margin-right: 0;}
.btn2{padding: 5px 10px;line-height: 1;border: 1px solid #e1e1e1;border-radius: 5px;}
</style>
