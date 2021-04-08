<template>
	<view>

		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb  bg-f5 shadow-sm nav position-fixed w-100 z-index1">
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">商务通</view>
			<view class="item" :class="navCurr==5?'on':''" @click="changeNav(5)">主页</view>
			
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">二手</view>
			
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">劳务</view>
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">资讯</view>
		</view>

		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-f5 choose" style="background-color: #fff;">
			<view @click="chooseShow" v-if="!isShowChoose">选择</view>
			<view @click="chooseShow" v-else>取消</view>
			<view class="Mcolor d-flex a-center line-h" v-if="isShowChoose">
				<view class="pr-4 borderR-e1" @click="chooseAll()">全选</view>
				<view class="pl-4" @click="deleteAll()">删除</view>
				
			</view>
		</view>

		<!-- 咨询 -->
		<view class="pt-3 bg-white borderB-f5" v-show="navCurr==1" v-for="(item,index) of mainData" :key="item.id">
			<view class="d-flex a-center px-3 pb-3"  v-if="item.style==2">
				<!-- <image src="../../static/images/i-releasel-icon.png" class="icon5"></image> -->
				<image @click="choose(index)" v-if="isShowChoose" 
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<view class="flex-1 ml-2" :data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="bg-white indexBox1 flex-1">
						<view class="d-flex a-center j-sb">
							<view class="d-flex flex-column j-sb mr-3 h-100">
								<view class="font-30 color2 flex-1 mb-3 avoidOverflow2 tjTit">{{item.title}}</view>
								<view class="d-flex a-center j-sb h-100 mt-3">
									<view class="font-22 d-flex a-center">
										<view class="tag tag1" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
										<view class="tag tag2" v-if="item.top==1">推荐</view>
									</view>
									<view class="d-flex a-center">
										<image src="../../static/images/home-icon.png" class="icon1"></image>
										<view class="font-24 color6 pl-1">{{item.view_count}}</view>
									</view>
								</view>
							</view>
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="tjImg"></image>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4" v-if="item.description!=''">
							
							<view class="font-24 color6 pl-1">{{item.description}}</view>
						</view>
					</view>
				</view>
			</view>

			<view class="d-flex a-center px-3 pb-3" v-if="item.style==1">
				<!-- <image src="../../static/images/i-releasel-icon.png" class="icon5"></image> -->
				<image @click="choose(index)" v-if="isShowChoose" 
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<view class="flex-1 ml-2 pt-3" :data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="bg-white indexBox1 flex-1">
						<view class="d-flex a-center j-sb">
							<view class="h-100 w-100">
								<view class="font-30 color2 flex-1 mb-3 pb-3 avoidOverflow2 tjTit">{{item.title}}</view>
								<view class="d-flex a-center j-sb h-100 mt-3">
									<view class="font-22 d-flex a-center">
										<view class="tag tag1" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
										<view class="tag tag2" v-if="item.top==1">推荐</view>
									</view>
									<view class="d-flex a-center">
										<image src="../../static/images/home-icon.png" class="icon1"></image>
										<view class="font-24 color6 pl-1">{{item.view_count}}</view>
									</view>
								</view>
							</view>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4" v-if="item.description!=''">
							
							<view class="font-24 color6 pl-1">{{item.description}}</view>
						</view>
					</view>
				</view>
			</view>

			<view class="d-flex a-center px-3 pb-3"  v-if="item.style==3">
				<!-- <image src="../../static/images/i-releasel-icon.png" class="icon5"></image> -->
				<image @click="choose(index)" v-if="isShowChoose" 
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<view class="flex-1 ml-2">
					<view class="bg-white indexBox2"  :data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="font-30 color2">{{item.title}}</view>
						<view class="d-flex a-center mt-3 flex-wrap imgBox">
							<image v-for="(c_item,c_index) in item.mainImg" :key="c_index" :src="c_item.url" mode=""></image>
						</view>
						<view class="d-flex a-center j-sb h-100 mt-3">
							<view class="font-22 d-flex a-center">
								<view class="tag tag1" v-for="(c_item,c_index) of item.keywords" :key="c_index">{{c_item}}</view>
								<view class="tag tag2" v-if="item.top==1">推荐</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/home-icon.png" style="margin-right: 0;" class="icon1"></image>
								<view class="font-24 color6 pl-1">{{item.view_count}}</view>
							</view>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4" v-if="item.description!=''">
							
							<view class="font-24 color6 pl-1">{{item.description}}</view>
						</view>
					</view>
				</view>
			</view>



			<view class="d-flex a-center px-3 pb-3 bg-white"  v-if="item.style==4">
				<image @click="choose(index)" v-if="isShowChoose" 
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<!-- <image src="../../static/images/i-releasel-icon1.png" class="icon5"></image> -->
				<view class="indexBox3 flex-1 ml-2">
					<view class="font-30 color2">{{item.title}}</view>
					<view class="mt-3">
						<video class="video" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" controls>
							
						</video>
					</view>
					<!-- <view class="d-flex a-center j-sb h-100 mt-3">
					
						<view class="d-flex a-center">
							<image src="../../static/images/home-icon.png" class="icon1"></image>
							<view class="font-24 color6 pl-1">{{item.view_count}}</view>
						</view>
					</view> -->
					<view class="bg-f5 d-flex a-center p-1 mt-4" v-if="item.description!=''">
						
						<view class="font-24 color6 pl-1">{{item.description}}</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 二手信息 -->
		<view class="list borderB-f5" v-show="navCurr==2" v-for="(item,index) of mainData" :key="item.id">
			<view class="d-flex a-center px-3  bg-white">
				<image @click="choose(index)" v-if="isShowChoose" 
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<!-- <image src="../../static/images/i-releasel-icon1.png" class="icon5"></image> -->
				<view class="item d-flex a-center j-sb py-3 pl-2 bg-white flex-1" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:(item.behavior==2?'../../static/images/qiugou.jpg':'')" mode="aspectFill"></image>
					<view class="itemCon flex-1 ml-2">
						<view class="color3 font-30 avoidOverflow2 tit">{{item.title}}</view>
						<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
							<view class="d-flex a-center">{{item.name}}
								<view class="tag tagName" v-if="item.user&&item.user[0]&&item.user[0].behavior==2">二手商</view>
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
		</view>


		<!-- 招聘信息 -->
		<view v-show="navCurr==4" class="bg-white borderB-f5" v-for="(item,index) of mainData" :key="item.id">
			<view class="d-flex a-center px-3 py-4">
				<image @click="choose(index)" v-if="isShowChoose"
				:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
				<!-- <image src="../../static/images/i-releasel-icon1.png" class="icon5"></image> -->
				<view class="pl-2 flex-1" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/labour-detail/labour-detail?id='+$event.currentTarget.dataset.id}})">
					<view class="font-30 color2">{{item.title}}</view>
					<view class="d-flex flex-wrap py-3 imgBox">
						<image v-for="(c_item,c_index) in item.mainImg" :key="c_index" :src="c_item.url" mode=""></image>

					</view>
					<view class="d-flex a-center j-sb">
						<view class="d-flex a-center">
							<view class="tag tagB" v-if="item.behavior==1">招工人</view>
							<view class="tag tagB" v-if="item.behavior==2">招队伍</view>
							<view class="tag tagG" v-if="item.behavior==3">工人找活</view>
							<view class="tag tagG" v-if="item.behavior==4">队伍找活</view>
							<view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view>
							<view class="d-flex a-center" v-if="item.price!=''">
								<span class="tag tagR" style="border-radius: 5rpx 0 0 5rpx;">介绍费</span>
								<span class="tag tagO" style="border-radius: 0 5rpx 5rpx 0;" >{{item.price}}</span>
							</view>
						</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
							<view class="font-24 color6 pl-1">{{item.city?item.city.title:''}}</view>
							<view class="font-24 color6 pl-2">{{Utils.formatMsgTime(item.update_time)}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>


		<view class="d-flex a-center px-3 bg-white"  v-show="navCurr==3" v-for="(item,index) of mainData" :key="item.id">
			<image @click="choose(index)" v-if="isShowChoose"
			:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
			<view class="rounded shadow m-2 px-2 bg-fc border-f5 pb-3 pl-2 flex-1" style="margin-right: 0;">
				<view class="Mcolor font-30 py-3">{{item.title}}</view>
				<view class="d-flex a-center j-sb line-h pb-3 borderB-de1">
					<view class="d-flex a-center"><text class="pr-2">{{item.name}}</text>
						<view class="mr-2" style="width: 1px;height: 28rpx;background-color: #e1e1e1;">
							
						</view>{{item.phone}}
					</view>
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
								<image @click="imgPreview(index,c_index)" v-for="(c_item,c_index) of item.mainImg" :key="c_index" :src="c_item.url" class="img"></image>
							</view>
						</view>
					</view>
					
				</view>
				<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
					<view class="btn Mborder rounded ml-5" @click="callPhone(index)">拨打电话</view>
				</view>
			</view>
		</view>
		
		<view  class="d-flex a-center bg-white px-3" v-show="navCurr==5"  v-for="(item,index) of mainData" :key="item.id">
			<image @click="choose(index)" v-if="isShowChoose"
			:src="item.choose?'../../static/images/i-releasel-icon1.png':'../../static/images/i-releasel-icon.png'" class="icon5"></image>
			<view class="bg-white py-4 d-flex a-start borderB-f5 px-2 flex-1"
			:data-id = "item.user_no"
			@click="Router.navigateTo({route:{path:'/pages/secondHand-detailCompany/secondHand-detailCompany?user_no='+$event.currentTarget.dataset.id}})">
				<view>
					<image :src="item.headImgUrl?item.headImgUrl:''" class="userImg"></image>
					<!-- <view class="Rcolor text-center font-18 pt-1">热度98561</view> -->
				</view>
				<view class="ml-3 flex-1">
					<view class="font-26 font-w color2 mb-2 flex-1 d-flex a-center">
						<view class="pr-4">{{item.info&&item.info.name?item.info.name:''}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon4.png" class="icon12"></image>
							<view>{{item.info&&item.info.phone?item.info.phone:''}}</view>
						</view>
					</view>
					<view class="font-24 color6">{{item.info&&item.info.passage2?item.info.passage2.substr(0,80):''}}</view>
				</view>
			</view>
		</view>
		
		<view style="height: 50rpx;"></view>

	</view>
</template>


<script>
	import Vue from 'vue'
	export default {
		data() {
			return {
				navCurr: 3,
				mainData: [],
				searchItem: {
					type: 1,
					behavior: 4,
					
				},
				searchItemTwo: {
					type: 2,
					user_type:['in',[0,2]]
				},
				Utils: this.$Utils,
				now: 0,
				isShowChoose:false,
				isChooseAll:false,
				allId:[],
				Router:this.$Router,
				testData:[]
			}
		},




		onLoad() {
			const self = this;
			self.now = Date.parse(new Date()) / 1000;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMessageData'], self);
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				if(self.navCurr==1){
					self.getMainData()
				}else{
					self.getMessageData()
				}
				
			};
		},

		methods: {
			
			
			
			
			deleteAll(index) {
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定取消收藏所选信息吗？',
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
										
										if(self.navCurr==1){
											self.getMainData(true)
										}else if(self.navCurr!=5){
											self.getMessageData(true)
										}else if(self.navCurr==5){
											self.getUserData(true)
										}
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(data.msg, 'none', 1000)
								}	
							};
							self.$apis.logUpdate(postData, callback);
						}
					}
				})
			},
			
			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				console.log('self.isChooseAll',self.isChooseAll)
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].choose = self.isChooseAll;
					//self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
					if(self.isChooseAll){
						self.allId.push(self.mainData[i].log[0].id)
					}else{
						self.allId = []
					}
				};
				//self.countTotalPrice();
			},
			
			choose(index){
				const self = this;
				console.log(index,self.mainData[index].choose)
				self.mainData[index].choose = !self.mainData[index].choose
				Vue.set(self.mainData,index,self.mainData[index])
				var options = self.allId.indexOf(self.mainData[index].log[0].id);
				if(options>=0){
					self.allId.splice(1,parseInt(options))
				}else{
					self.allId.push(self.mainData[index].log[0].id)
				}
				
			},
			
			chooseShow(){
				const self = this;
				self.isShowChoose = !self.isShowChoose
			},

			callPhone(index) {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.mainData[index].phone
				})
			},

			changeNav(i) {
				const self = this;
				if (self.navCurr != i) {
					self.navCurr = i;
					if (self.navCurr == 1) {
						self.getMainData(true)
					} else if(self.navCurr!=5){
						self.searchItemTwo.type = parseInt(self.navCurr) - 1;
						self.getMessageData(true)
					}else if(self.navCurr==5){
						self.getUserData(true)
					}
				}
			},
			
			getUserData(isNew) {
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
				postData.searchItem = {
					thirdapp_id:2,
					//user_type:0
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.getBefore = {
					log: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'user_no',
						key: 'relation_id',
						searchItem: {
							relation_table: ['in', ['User']],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						condition: 'in'
					}
				};
				postData.getAfter = {
					log: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'user_no',
						key: 'relation_id',
						searchItem: {
							relation_table: 'User'				
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
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
				postData.searchItem = {
					thirdapp_id:2,
					//user_type:0
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.getBefore = {
					log: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							relation_table: ['in', ['Article']],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						condition: 'in'
					}
				}
				postData.getAfter = {
					log: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							relation_table: 'Article'
				
						},
						condition: '='
					}
				}
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].keywords = self.mainData[i].keywords.split(',')
							self.mainData[i].choose = false
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

			getMessageData(isNew) {
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItemTwo);
				//postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getBefore = {
					log: {
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							relation_table: ['in', ['Message']],
							user_no:['in',[uni.getStorageSync('user_info').user_no]]
						},
						condition: 'in'
					}
				}
				postData.getAfter = {
					relationLog: {
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							relation_table: 'Article'
									
						},
						condition: '='
					},
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
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].choose = false
						}
					};
					self.$Utils.finishFunc('getMessageData');
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
	.icon12 {
		width: 20rpx!important;
		height: 26rpx!important;
		margin-right: 10rpx;
	}
	.shadow {
		
		box-shadow: 2rpx 3rpx 3rpx 0rpx 
				rgba(0, 0, 0, 0.16)!important;
	}
	.tagR{margin-right: 0;}
	.nav {
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
	}

	.nav .item {
		width: 25%;
		line-height: 90rpx;
		text-align: center;
	}
	.userImg {
		width: 100rpx;
		height: 100rpx;
	}
	.nav .on {
		position: relative;
		color: #51A9E9;
	}

	.nav .on::before {
		content: '';
		width: 100%;
		height: 4rpx;
		background-color: #51A9E9;
		position: absolute;
		bottom: 0;
		left: 0;
	}

	.choose {
		margin-top: 90rpx;
	}

	.icon1 {
		width: 33rpx !important;
		height: 23rpx !important;
	}

	/* .tjTit{width: 480rpx;} */
	.tjImg {
		width: 210rpx;
		height: 160rpx;
	}

	.icon2 {
		width: 27rpx !important;
		height: 27rpx !important;
	}
	.icon3 {
		width: 21rpx!important;
		height: 25rpx!important;
	}
	.icon4 {
		width: 21rpx;
		height: 25rpx;
	}

	.icon5 {
		width: 40rpx;
		height: 40rpx;
	}

	.video {
		height: 340rpx;
		width: 100%;
	}

	.list .item image {
		width: 180rpx;
		height: 180rpx;
	}

	.list .itemCon .tit {
		/* width: 480rpx; */
		line-height: 1.2;
	}

	.list .itemConL {
		width: 180rpx;
		height: 180rpx;
		line-height: 180rpx;
		background-color: #9DD6FF;
	}

	.imgBox image {
		width: 145rpx;
		height: 145rpx;
		margin-right: 19rpx;
	}

	.imgBox image:nth-child(4n) {
		margin-right: 0;
	}

	.indexBox2 image {
		width: 192rpx;
		height: 160rpx;
		margin-right: 27rpx;
	}

	.indexBox2 image:nth-child(3n) {
		margin-right: 0;
	}

	.btn {
		width: 120rpx;
		line-height: 40rpx;
	}

	.img {
		width: 106rpx;
		height: 106rpx;
		margin-top: 20rpx;
		margin-right: 20rpx;
	}

	.img:nth-child(4n) {
		margin-right: 0;
	}
	.bg-f5 {
		background-color: #f5f5f5;
	}
</style>
