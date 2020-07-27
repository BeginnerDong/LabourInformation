<template>
	<view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">咨询</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">二手信息</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">商务通</view>
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">招聘信息</view>
			
		</view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-e1 choose">
			<view>选择</view>
			<view class="Mcolor borderR-f5" @click="showToast()">删除</view>
		</view>
		 
		<!-- 咨询 -->
		<view class="pt-3 bg-white" v-show="navCurr==1">
			<view class="d-flex a-center px-3 pb-3 borderB-e1">
				<!-- <image src="../../static/images/i releasel-icon.png" class="icon5"></image> -->
				<image src="../../static/images/i releasel-icon1.png" class="icon5"></image>
				<view class="flex-1 ml-2">
					<view class="bg-white indexBox1 flex-1">
						<view class="d-flex a-center j-sb">
							<view class="d-flex flex-column j-sb mr-3 h-100">
								<view class="font-30 color2 flex-1 mb-3 avoidOverflow2 tjTit">隧道佳乐湿喷机，多年口碑，品质保证</view>
								<view class="d-flex a-center j-sb h-100 mt-3">
									<view class="font-22 d-flex a-center">
										<view class="tag tag1">导购</view>
										<view class="tag tag1">湿喷机</view>
										<view class="tag tag2">推荐</view>
									</view>
									<view class="d-flex a-center">
										<image src="../../static/images/home-icon.png" class="icon1"></image>
										<view class="font-24 color6 pl-1">252</view>
									</view>
								</view>
							</view>
							<image src="../../static/images/home-img.png" class="tjImg"></image>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4">
							<image src="../../static/images/home-icon2.png" class="icon2"></image>
							<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="d-flex a-center px-3 pb-3 borderB-e1">
				<!-- <image src="../../static/images/i releasel-icon.png" class="icon5"></image> -->
				<image src="../../static/images/i releasel-icon1.png" class="icon5"></image>
				<view class="flex-1 ml-2 pt-3">
					<view class="bg-white indexBox1 flex-1">
						<view class="d-flex a-center j-sb">
							<view class="h-100 w-100">
								<view class="font-30 color2 flex-1 mb-3 pb-3 avoidOverflow2 tjTit">隧道佳乐湿喷机，多年口碑，品质保证</view>
								<view class="d-flex a-center j-sb h-100 mt-3">
									<view class="font-22 d-flex a-center">
										<view class="tag tag1">导购</view>
										<view class="tag tag1">湿喷机</view>
										<view class="tag tag2">推荐</view>
									</view>
									<view class="d-flex a-center">
										<image src="../../static/images/home-icon.png" class="icon1"></image>
										<view class="font-24 color6 pl-1">252</view>
									</view>
								</view>
							</view>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4">
							<image src="../../static/images/home-icon2.png" class="icon2"></image>
							<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="d-flex a-center px-3 pb-3 borderB-e1">
				<!-- <image src="../../static/images/i releasel-icon.png" class="icon5"></image> -->
				<image src="../../static/images/i releasel-icon1.png" class="icon5"></image>
				<view class="flex-1 ml-2">
					<view class="pt-3 bg-white indexBox2" 
					@click="Router.navigateTo({route:{path:'/pages/newsDetail/newsDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="font-30 color2">隧道佳乐湿喷机，多年口碑，品质保证</view>
						<view class="d-flex a-center mt-3 flex-wrap imgBox">
							<image src="../../static/images/home-img.png" mode=""></image>
							<image src="../../static/images/home-img.png" mode=""></image>
							<image src="../../static/images/home-img.png" mode=""></image>
						</view>
						<view class="d-flex a-center j-sb h-100 mt-3">
							<view class="font-22 d-flex a-center">
								<view class="tag tag1">批发</view>
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/home-icon.png" class="icon1"></image>
								<view class="font-24 color6 pl-1">565985</view>
							</view>
						</view>
						<view class="bg-f5 d-flex a-center p-1 mt-4">
							<image src="../../static/images/home-icon2.png" class="icon2"></image>
							<view class="font-24 color6 pl-1">6265</view>
						</view>
					</view>
					</view>
				</view>
				
			</view>
			
			<view class="d-flex a-center px-3 pb-3 bg-white">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="indexBox3 flex-1 ml-2 pt-3">
					<view class="font-30 color2">厂家直销各种胶皮手套、818止滑手套、线手套、电焊手套等各种施工手套</view>
					<view class="mt-3">
						<video class="video" controls poster="../../static/images/home-img2.png">
						    <source src="" type="video/mp4">
						    您的浏览器不支持 video 标签。
						</video>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-24 color6">时长：12分钟</view>
						<view class="d-flex a-center">
							<image src="../../static/images/home-icon.png" class="icon1"></image>
							<view class="font-24 color6 pl-1">252</view>
						</view>
					</view>
					<view class="bg-f5 d-flex a-center p-1 mt-4">
						<image src="../../static/images/home-icon2.png" class="icon2"></image>
						<view class="font-24 color6 pl-1">性价比较高，可以对比一下其他品牌</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 二手信息 -->
		<view class="list" v-show="navCurr==2" v-for="(item,index) of mainData" :key="id">
			<view class="d-flex a-center px-3 pb-3 bg-white">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="item d-flex a-center j-sb py-3 pl-2 bg-white flex-1" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/secondHand-detail/secondHand-detail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					<view class="itemCon flex-1 ml-2">
						<view class="color3 font-30 avoidOverflow2 tit">{{item.title}}</view>
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
		</view>

		
		<!-- 招聘信息 -->
		<view v-show="navCurr==4" class="bg-white" v-for="(item,index) of mainData"
		:key="item.id" >
			<view class="d-flex a-center px-3">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="pl-2 flex-1" :data-id="item.id"
		@click="Router.navigateTo({route:{path:'/pages/labour-detail/labour-detail?id='+$event.currentTarget.dataset.id}})">
					<view class="font-30 color2 pt-4">{{item.title}}</view>
					<view class="d-flex flex-wrap py-3 imgBox">
						<image v-for="(c_item,c_index) in item.mainImg" :key="c_index"  :src="c_item.url" mode=""></image>
						
					</view>
					<view class="d-flex a-center j-sb">
						<view>
							<view class="tag tagB" v-if="item.behavior==1">招工人</view>
							<view class="tag tagB" v-if="item.behavior==2">招队伍</view>
							<view class="tag tagG" v-if="item.behavior==3">工人找活</view>
							<view class="tag tagG" v-if="item.behavior==4">队伍找活</view>
							<view class="tag tagY" v-if="item.invalid_time<now">信息已失效</view>
							<view v-if="item.price!=''"><text class="tag tagR">介绍费</text><text class="tag tagO">{{item.price}}</text></view>
						</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
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
			</view>
		</view>

		<!-- 商务通 -->
		<view v-show="navCurr==3" class="bg-white" v-for="(item,index) of mainData" :key="item.id">
			<view  class="d-flex a-center px-3">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="flex-1 ml-2">
					<view class="Mcolor font-30 py-3">{{item.title}}</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
						<view><text class="borderR-e1 pr-2 mr-2">{{item.name}}</text>{{item.phone}}</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
							<view class="font-24 color6 pl-1">{{item.city?item.city.title:''}}</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">
							<view>{{item.description}}</view>
							<view class="d-flex a-center flex-wrap" v-if="item.mainImg.length>0">
								<image v-for="(c_item,c_index) of item.mainImg" :key="c_index" :src="item.url" class="img"></image>
							</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">销售区域：</view>
						<view class="color2 mr-1" :key="c_item.id" v-for="(c_item,c_index) in item.relation">{{c_item.relation_two}}</view>
					</view>
					<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
						<view class="btn Mborder rounded ml-5"  @click="callPhone(index)">拨打电话</view>
					</view>
					<view class="font-22 color2 py-3" @click="Router.navigateTo({route:{path:'/pages/user-opinion/user-opinion'}})">不实信息投诉</view>
				</view>
			</view>
		</view>
		
		
		<view style="height: 50rpx;"></view>
		
	</view>
</template>


<script>
	export default {
		data() {
			return {
				navCurr:1,
				mainData:[],
				searchItem:{
					type:1,
					behavior:1
				},
				searchItemTwo:{
					type:1,
					type:0
				},
				Utils:this.$Utils,
				now:0
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
			
			callPhone(index) {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.mainData[index].phone
				})
			},
			
			changeNav(i){
				const self = this;
				if(self.navCurr!=i){
					self.navCurr = i;
					if(self.navCurr==1){
						self.getMainData(true)
					}else{
						self.searchItemTwo.type =parseInt(self.navCurr) - 1;
						self.getMessageData(true)
					}
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
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
				}	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
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
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getBefore = {
					log:{
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							relation_table:['in',['Message']]
							
						},
						condition:'in'
					}
				}
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
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.nav{position: fixed;top: 0;right: 0;left: 0;}
.nav .item{width: 25%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}
.choose{margin-top: 90rpx;}

.icon1{width: 30rpx!important;height: 22rpx!important;}
/* .tjTit{width: 480rpx;} */
.tjImg{width: 210rpx;height: 160rpx;}
.icon2{width: 27rpx!important;height: 27rpx!important;}
.icon4{width: 21rpx;height: 25rpx;}
.icon5{width: 40rpx;height: 40rpx;}
.video{height: 340rpx;width: 100%;}

.list .item image{width: 180rpx;height: 180rpx;}
.list .itemCon .tit{/* width: 480rpx; */line-height: 1.2;}
.list .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.imgBox image{width: 145rpx;height: 145rpx;margin-right: 19rpx;}
.imgBox image:nth-child(4n){margin-right: 0;}
.indexBox2 image{width: 192rpx;height: 160rpx;margin-right: 31rpx;}
.indexBox2 image:nth-child(3n) {margin-right: 0;}

.btn{width: 120rpx;line-height: 40rpx;}
.img{width: 106rpx;height: 106rpx;margin-top: 20rpx;margin-right: 20rpx;}
.img:nth-child(4n){margin-right: 0;}
</style>
