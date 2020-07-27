<template>
	<view>
		<view class="text-center font-24 color2 p-3 head position-relative" v-show="tips_show">
			<view class="Rcolor pb-3">温馨提示</view>
			<view class="pb-1" style="text-indent: 40rpx;">
				<text class="Rcolor">如果您是工程高管或老板，</text>
				请添加劳务信息官方微信：hjhsn2500（免费认证），以后有招工人/找班组需求告知我们完全免费帮您发布（不收取任何费用）
			</view>
			<view class="close p-3" @click="changeTips()"><image src="../../static/images/used%20to%20releasel-icon2.png" class="icon1"></image></view>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">招工人</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">招队伍</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">工人找活</view>
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">队伍找活</view>
		</view>
		
		<!-- 添加图片 -->
		<view class="upload font-24 color2 px-3 py-4 bg-white mb-2">
			<view>添加图片（最多5张图片,选填）</view>
			<view class="uploadImg d-flex a-center">
				<view class="position-relative" v-for="(item,index) of submitData.mainImg" :key="index">
					<image :src="item.url" mode=""></image>
					<image src="../../static/images/used to releasel-icon1.png" @click="deleteImg(index)" class="icon2"></image>
				</view>
				<image v-if="submitData.mainImg.length<5" @click="upLoadImg('mainImg')" src="../../static/images/used to releasel-icon.png"
				 mode=""></image>
			</view>
		</view>
		<!-- 列表信息 -->
		<view class="bg-white">
			<view class="py-4 borderB-f5 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28" v-show="navCurr==1">标题/招工人需求</view>
					<view class="font-28" v-show="navCurr==2">标题/招班组需求</view>
					<view class="font-28" v-show="navCurr==3">标题/您的应聘内容</view>
					<view class="font-28" v-show="navCurr==4">标题/您的队伍简介</view>
					<view class="font-24">({{submitData.title.length}}/100)</view>
				</view>
				<textarea  maxlength="100" placeholder="请填写100个字以内的内容"  v-model="submitData.title"/>
			</view>
			
			<!-- ---------工人找活展示------- -->
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3">
				<view class="font-28 color2">求职岗位</view>
				<input type="text" placeholder="请填写" v-model="submitData.description"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3">
				<view class="font-28 color2">期望薪资</view>
				<input type="text" placeholder="请填写" v-model="submitData.salary"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3 || navCurr==4">
				<view class="font-28 color2">户籍所在地</view>
				<view class="d-flex a-center" @click="showChoose('city')">
					<view class="font-24 color9 pr-1">{{submitData.location!=''?
					cityData[cityIndex].title+'/'+cityData[cityIndex].children[cityIdIndex].title:'请选择'}}</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			<!-- ------------------------------- -->
			
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==1 || navCurr==2">
				<view class="font-28 color2">工程所在地区</view>     
				<view class="d-flex a-center" @click="showChoose('city')">
					<view class="font-24 color9 pr-1">{{submitData.location!=''?
					cityData[cityIndex].title+'/'+cityData[cityIndex].children[cityIdIndex].title:'请选择'}}</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">联系人</view>
				<input type="text" placeholder="请填写" v-model="submitData.name"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">手机号</view>
				<input type="text" placeholder="请填写" v-model="submitData.phone"/>
			</view>
		</view>
		
		
		<view class="card1 bg-white">
			<!-- 招工人 -->
			<view class="px-3 mt-2" v-show="navCurr==1">
				<view class="color2 font-28 d-flex a-center">
					<view class="pt-4 pb-3 select">招聘工种</view>
					<view class="pt-4 pb-3 select1">人数</view>
					<view class="pt-4 pb-3 select1">月工资</view>
				</view>
				<view class="d-flex a-center pb-2" v-for="(item,index) of worker" :key="index">	
					<input type="text" placeholder="请填写" v-model="item.name" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<input type="number" placeholder="请填写" v-model="item.num" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<input type="text" placeholder="不填为面议" v-model="item.money" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<view class="font-28 Rcolor" v-if="index>0" @click="deleteWorker(index)">删除</view>
				</view>
				<view class="d-flex a-center j-center py-4" @click="addWorker">
					<image src="../../static/images/labor%20releasel-icon.png" class="icon3"></image>
					<view class="pl-2 font-26 Mcolor">继续添加</view>
				</view>
			</view>
			<!-- 找队伍 -->
			<view class="px-3 mt-2" v-show="navCurr==2">
				<view class="color2 font-28 d-flex a-center">
					<view class="pt-4 pb-3 select">招聘工种</view>
					<view class="pt-4 pb-3 select1">队伍数量</view>
					<view class="pt-4 pb-3 select1">待遇</view>
				</view>
				<view class="d-flex a-center pb-2" v-for="(item,index) of team" :key="index">
					<input type="text" placeholder="请填写" v-model="item.name" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<input type="number" placeholder="请填写" v-model="item.num" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<input type="text" placeholder="不填为面议" v-model="item.money" class="text-left border-e1 rounded color2 pl-1 borderBox select1"/>
					<view class="font-28 Rcolor" v-if="index>0" @click="deleteTeam(index)">删除</view>
				</view>
				<view class="d-flex a-center j-center py-4" @click="addTeam">
					<image src="../../static/images/labor%20releasel-icon.png" class="icon3"></image>
					<view class="pl-2 font-26 Mcolor">继续添加</view>
				</view>
			</view>
			
			<view class="px-3 mt-2" v-show="navCurr==4">
				<view class="color2 font-28 d-flex a-center">
					<view class="pt-4 pb-3">您拥有的队伍</view>
					<view class="pt-4 pb-3" style="font-size: 12px;">（最多添加3个队伍信息）</view>
				</view>
				<view class="d-flex a-center pb-2" v-for="(item,index) of myTeam" :key="index">
					<input type="text" placeholder="请填写" v-model="item.name" class="text-left border-e1 rounded color2 pl-1 borderBox"/>
					<view class="font-28 Rcolor" v-if="index>0" @click="deleteMyTeam(index)">删除</view>
				</view>
				<view class="d-flex a-center j-center py-4" @click="addMyTeam">
					<image src="../../static/images/labor%20releasel-icon.png" class="icon3"></image>
					<view class="pl-2 font-26 Mcolor">继续添加</view>
				</view>
			</view>
		</view>
		
		
		<!-- 介绍费 -->
		<view class="mt-2 bg-white pb-5">
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">介绍费</view>
				<input type="text" placeholder="请输入金额,可不填写" v-model="submitData.price"/>
			</view>
			<view class="d-flex p-3 mb-5">
				<view class="font-22 Rcolor">设置介绍说明：</view>
				<view class="font-22 color6 line-h-md">
					<view><text class="number">1</text>你可以设置介绍费，促进他人帮你介绍</view>
					<view><text class="number">2</text>他人帮您介绍成功</view>
					<view><text class="number">3</text>您直接支付介绍着的介绍费(介绍费不用交给平台)</view>
				</view>
			</view>
			<view class="btn400" @click="Utils.stopMultiClick(submit)">确认发布</view>
		</view>
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0" v-show="city">
			<view>
				<view class="d-flex a-center j-sb px-3 py-2 borderB-e1 bg-white">
					<view class="font-26 Mcolor d-flex a-center" @click="showChoose('city')"><image src="../../static/images/used to releasel-icon4.png" class="icon4"></image>返回</view>
					<view class="bg-mcolor colorf rounded10 yesBtn" @click="confirmCity()">确认</view>
				</view>
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
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tips_show:true,
				navCurr:1,
				submitData:{
					type:3,
					behavior:1,
					title:'',
					location:'',
					name:'',
					phone:'',
					passage_array:[],
					salary:'',
					mainImg:[],
					description:''
				},
				cityData:[],
				cityIndex:0,
				cityIdIndex:-1,
				city:false,
				worker:[
					{
						name:'',
						num:'',
						money:''
					}
				],
				team:[
					{
						name:'',
						num:'',
						money:''
					}
				],
				myTeam:[
					{
						name:''
					}
				],
				Utils:this.$Utils,
				isEdit:false
			}
		},
		
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getCityData'], self);
			if(options.id){
				self.isEdit = true
				self.getMessageData(options.id)
			}
		},
		
		methods: {
			
			getMessageData(id) {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:id,
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData = res.info.data[0];
						self.submitData.title = self.messageData.title;
						self.submitData.name = self.messageData.name;
						self.submitData.phone = self.messageData.phone;
						self.submitData.mainImg = self.messageData.mainImg;
						self.submitData.behavior = self.messageData.behavior;
						self.submitData.location = self.messageData.location;
						self.submitData.passage_array = self.messageData.passage_array;
						self.submitData.salary = self.messageData.salary;
						self.submitData.description = self.messageData.description;
						//console.log(self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location))
						self.cityIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[0];
						self.cityIdIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[1];
						if(self.submitData.behavior == 1){
							self.worker = self.submitData.passage_array
						}else if(self.submitData.behavior == 2){
							self.team = self.submitData.passage_array
						}else if(self.submitData.behavior == 4){
							self.myTeam = self.submitData.passage_array
						}
					}
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
			

			
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.behavior==1){
					self.submitData.passage_array = self.worker
				}else if(self.submitData.behavior==2){
					self.submitData.passage_array = self.team
				}else if(self.submitData.behavior==4){
					self.submitData.passage_array = self.myTeam
				};
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.mainImg;
				delete newObject.price;
				
				if(self.submitData.behavior!=3){
					delete newObject.salary;
					delete newObject.description
				}else{
					delete newObject.passage_array
				};
				const pass = self.$Utils.checkComplete(newObject);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入真实有效的手机号', 'none', 1000)
						return;
					}
					if(self.isEdit){
						self.messageUpdate()
					}else{
						self.messageAdd();
					}
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全必须信息', 'none')
				};
			},
			
			messageUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.searchItem = {
					id:self.messageData.id
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('修改成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageUpdate(postData, callback);
			},
			
			messageAdd() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发布成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			addWorker(){
				const self = this;
				self.worker.push({
					name:'',
					num:'',
					money:''
				})
			},
			
			deleteWorker(index){
				const self = this;
				self.worker.splice(parseInt(index),1)
			},
			
			addTeam(){
				const self = this;
				self.team.push({
					name:'',
					num:'',
					money:''
				})
			},
			
			deleteTeam(index){
				const self = this;
				self.team.splice(parseInt(index),1)
			},
			
			addMyTeam(){
				const self = this;
				self.myTeam.push(
					{
						name:''
					}
				)
			},
			
			deleteMyTeam(index){
				const self = this;
				self.myTeam.splice(parseInt(index),1)
			},
			
			showChoose(e){
				const self = this;
				self[e] = !self[e];
			},
			
			changeCityIndex(index){
				const self = this;
				self.cityIndex = index
			},
			
			
			chooseCityId(index){
				const self = this;
				self.cityIdIndex = index
			},
			
			confirmCity(){
				const self = this;
				self.submitData.location = self.cityData[self.cityIndex].children[self.cityIdIndex].id;
				self.city = false;
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
			
			changeTips(){
				const self = this;
				self.tips_show = false
			},
			
			changeNav(i){
				const self = this;
				self.navCurr = i;
				self.submitData.behavior = self.navCurr
			},
			
			upLoadImg(type) {
				const self = this;			
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						//self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 5-self.submitData.mainImg.length,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			deleteImg(e){
				const self = this;
				self.submitData.mainImg.splice(parseInt(e),1)
			},
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.borderBox{box-sizing: border-box;}
.head{background-color: #FFF4D4;}
.icon1{width: 19rpx;height: 18rpx;}
.close{position: absolute;top: 0;right: 0;}

.nav image{width: 13rpx;height: 6rpx;margin-left: 8rpx;}
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon2Box{position: absolute;right: 0;top: 10rpx;padding: 10rpx;}
.icon2{width: 18rpx;height: 10rpx;}
.select{width: 220rpx;line-height: 60rpx;text-indent: 10rpx;margin-right: 30rpx;}
.select1{width: 160rpx;line-height: 60rpx;margin-right: 30rpx;}
.card1 .on{border: 1px solid #51A9E9;}


.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}

.icon3{width: 22rpx;height: 22rpx;}
.icon4{width: 12rpx;height: 22rpx;}
.number{border: 1px solid #666;color: #666;border-radius: 50%;width: 24rpx;display: inline-block;line-height: 1;text-align: center;margin-right: 10rpx;margin-bottom: 20rpx;}
</style>
