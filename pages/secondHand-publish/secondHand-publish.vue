<template>
	<view>
		<view class="text-center font-24 color2 line-h p-3 head position-relative" v-show="tips_show">
			<view class="Rcolor pb-3">温馨提示</view>
			<view class="pb-1"><text class="Rcolor">自己发布太麻烦？</text>请添加劳务信息官方微信：{{kefu}}</view>
			<view class="pb-1">我们完全免费帮您发布（平台不收取任何费用）</view>
			<view>劳务信息有强大的资源渠道，能够帮您快速的出售/求购</view>
			<view class="close p-3" @click="changeTips()">
				<image src="../../static/images/used%20to%20releasel-icon2.png" class="icon1"></image>
			</view>
		</view>

		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 shadow-sm bg-white nav">
			<view class="item" :class="submitData.behavior==1?'on':''" @click="changeBehavior(1)">发布出售</view>
			<view class="item" :class="submitData.behavior==2?'on':''" @click="changeBehavior(2)">发布需求</view>
		</view>

		<!-- 添加图片 -->
		<view class="upload font-24 color2 px-3 py-4 bg-white mb-2">
			<view v-if="submitData.behavior==1">添加图片（最多上传5张图片，至少上传一张图片）</view>
			<view v-if="submitData.behavior==2">添加图片（最多上传5张图片，选填）</view>
			<view class="uploadImg d-flex a-center" style="flex-wrap: wrap;">
				<view class="position-relative" v-for="(item,index) of submitData.mainImg" :key="index">
					<image :src="item.url" mode=""></image>
					<image src="../../static/images/used-to-releasel-icon1.png" @click="deleteImg(index)" class="icon2"></image>
				</view>
				<image v-if="submitData.mainImg.length<5" @click="upLoadImg('mainImg')" src="../../static/images/used-to-releasel-icon.png"
				 mode=""></image>
			</view>
		</view>

		<view class="bg-white">
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">分类</view>
				<view class="d-flex a-center" @click="showChoose('menu')">
					<view class="font-24 color9 pr-1">{{submitData.menu_id!=''?
					menuData[menuIndex].title+'/'+menuData[menuIndex].children[menuIdIndex].title:'请选择'}}</view>
					<image src="../../static/images/used-to-releasel-icon3.png" class="icon3"></image>
				</view>
			</view>
			<view class="py-4 borderB-e1 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28">标题/求购描述</view>
					<view class="font-24">({{submitData.title.length}}/100)</view>
				</view>
				<textarea value="" maxlength="100" placeholder="请填写100个字以内的内容" v-model="submitData.title" />
				</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">出售价格</view>
				<input type="text" placeholder="此项不填为面议" v-model="submitData.price"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">所在地区</view>
				<view class="d-flex a-center" @click="showChoose('city')">
					<view class="font-24 color9 pr-1">{{submitData.location!=''?
					cityData[cityIndex].title+'/'+cityData[cityIndex].children[cityIdIndex].title:'请选择'}}</view>
					<image src="../../static/images/used-to-releasel-icon3.png" class="icon3"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">联系人</view>
				<input type="text" placeholder="请填写" v-model="submitData.name"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">手机号</view>
				<input type="number" placeholder="请填写" v-model="submitData.phone"/>
			</view>
		</view>
		
		<view class="bg-white py-5" @click="Utils.stopMultiClick(submit)">
			<view class="btn400">{{!isEdit?'确认发布':'确认修改'}}</view>
		</view>
		
		
		
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0" v-show="menu||city">
			
			<!-- 分类 -->
			<view v-show="menu">
				
				<view class="d-flex a-center j-sb px-3 py-2 borderB-e1 bg-white">
					<view class="font-26 Mcolor d-flex a-center" @click="showChoose('menu')"><image src="../../static/images/used-to-releasel-icon4.png" class="icon4"></image>返回</view>
					<view class="bg-mcolor colorf rounded10 yesBtn" @click="confirmMenu()">确认</view>
				</view>
				
				<view class="Rcolor px-3 py-2 bg-white font-22 text-center">没有您要的分类请联系客服进行增加分类（微信号：{{kefu}}）</view>
				<view class="classfiy font-26 color2 line-h text-center d-flex">
					<view class="left">
						<view class="li py-3" v-for="(item,index) of menuData" :key="item.id"
						 @click="changeMenuIndex(index)" :class="menuIndex==index?'on':''">{{item.title}}</view>
					</view>
					<view class="right flex-1 bg-white">
						<view class="li" :class="menuIdIndex==index?'on':''" @click="chooseMenuId(index)" v-for="(item,index) of menuData[menuIndex].children" 
						:key="item.id">{{item.title}}
							<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="menuIdIndex==index"></image>
						</view>
					</view>
				</view>
			</view>
			
			<view  v-show="city">
				<view class="d-flex a-center j-sb px-3 py-2 borderB-e1 bg-white">
					<view class="font-26 Mcolor d-flex a-center" @click="showChoose('city')"><image src="../../static/images/used-to-releasel-icon4.png" class="icon4"></image>返回</view>
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
							<image src="../../static/images/used-to-releasel-icon5.png" class="icon5" v-if="cityIdIndex==index"></image>
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
				Router:this.$Router,
				tips_show:true,
				classCurr:1,
				addCurr:1,
				submitData:{
					type:1,
					title:'',
					menu_id:'',
					mainImg:[],
					price:'',
					location:'',
					name:'',
					phone:'',
					behavior:1,
					invalid_time:'',
					thirdapp_id:2
				},
				menuData:[],
				cityData:[],
				menu:false,
				city:false,
				menuIndex:0,
				menuIdIndex:-1,
				cityIndex:0,
				cityIdIndex:-1,
				Utils:this.$Utils,
				isEdit:false,
				kefu:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData','getMenuData','getCityData'], self);
			self.kefu = uni.getStorageSync('kefu');
			if(options.id){
				self.isEdit = true
				self.getMessageData(options.id)
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.submitData.invalid_time = Date.parse(new Date()) / 1000 + 7*86400 ;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.price;
				if(self.submitData.bahavior==2){
					delete newObject.mainImg;
				};
				const pass = self.$Utils.checkComplete(newObject);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入真实有效的手机号', 'none', 1000)
						return;
					};
					if(self.isEdit){
						self.messageUpdate()
					}else{
						self.messageAdd();
					}
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							name:self.submitData.name,
							phone:self.submitData.phone
						},
					},
				];
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发布成功', 'none', 1000)
						setTimeout(function() {
							self.Router.redirectTo({route:{path:'/pages/publish-success/publish-success?id='+data.info.id}})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			messageUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.searchItem = {
					id:self.messageData.id
				};
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							name:self.submitData.name,
							phone:self.submitData.phone
						},
					},
				];
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
			
			changeMenuIndex(index){
				const self = this;
				self.menuIndex = index
			},
			
			chooseMenuId(index){
				const self = this;
				self.menuIdIndex = index
			},
			
			confirmMenu(){
				const self = this;
				self.submitData.menu_id = self.menuData[self.menuIndex].children[self.menuIdIndex].id;
				self.menu = false;
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
			
			showChoose(e){
				const self = this;
				self[e] = !self[e];
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
						var tempFilePaths = res.tempFilePaths;
						for (var i = 0; i < tempFilePaths.length; i++) {
							var file = res.tempFiles[i];
							var obj = res.tempFiles[i].path.lastIndexOf(".");
							var ext = res.tempFiles[i].path.substr(obj+1);
							console.log(callback)
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
							}, callback)
						}
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
			
			changeBehavior(e){
				const self = this;
				self.submitData.behavior = e
			},
			
			getUserInfoData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.submitData.name = self.userInfoData.name;
						self.submitData.phone = self.userInfoData.phone
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
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
			
			changeTips(){
				const self = this;
				self.tips_show = false
			},
			
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
						self.submitData.menu_id = self.messageData.menu_id;
						self.submitData.location = self.messageData.location;
						//console.log(self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location))
						self.cityIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[0];
						self.cityIdIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[1];
						self.menuIndex = self.$Utils.findItemInTwoArray(self.menuData,self.submitData.menu_id)[0];
						self.menuIdIndex = self.$Utils.findItemInTwoArray(self.menuData,self.submitData.menu_id)[1];
					}
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},

		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.head{background-color: #FFF4D4;}
.icon1{width: 19rpx;height: 18rpx;}
.close{position: absolute;top: 0;right: 0;}

.nav .item{width: 50%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #55aae9;position: absolute; bottom: 0;left: 0;}

.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}
</style>
