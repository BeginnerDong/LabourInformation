<template>
	<view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">基本信息</view>
			<view class="item" :class="navCurr==2?'on':''" @click="submit">名片类型</view>
		</view>
		
		<!-- 基本信息 -->
		<view v-show="navCurr==1" class="pb-5">
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">公司名称</view>     
				<input type="text" placeholder="请填写" v-model="submitData.title"/>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">所在地区</view>     
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
				<input type="text" placeholder="请填写" v-model="submitData.phone" @blur="checkPhone" />
			</view>
			
			<view class="py-4 borderB-f5 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28">业务范围</view>
					<view class="font-24">({{submitData.description.length}}/100)</view>
				</view>
				<textarea value="" maxlength="100" placeholder="请填写100个字以内的内容" v-model="submitData.description"/>
			</view>
			
			<!-- 添加图片 -->
			<view class="upload font-24 color2 px-3 py-4 bg-white borderB-f5">
				<view>添加图片（最多上传3张图片，选填）</view>
				<view class="uploadImg d-flex a-center">
					<view class="position-relative" v-for="(item,index) of submitData.mainImg" :key="index">
						<image :src="item.url" mode=""></image>
						<image src="../../static/images/used to releasel-icon1.png" @click="deleteImg(index)" class="icon2"></image>
					</view>
					<image v-if="submitData.mainImg.length<5" @click="upLoadImg('mainImg')" src="../../static/images/used to releasel-icon.png"
					 mode=""></image>
				</view>
			</view>
			
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">销售区域（可多选)</view>     
				<view class="d-flex a-center" @click="showChoose('saleCity')">
					<view class="font-24 color9 pr-1">{{conSalecityTitleArray.length>0?conSalecityTitleArray[0]+'等':'不填写默认全国'}}</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			
			<view class="my-5" @click="submit">
				<view class="btn400">下一步</view>
			</view>
		</view>
		
		<!-- 名片类型 -->
		<view class="" v-show="navCurr==2">
			<view class="font-24 color2 font-w text-center py-4">请选择名片分类（最多选择5个）</view>
			<view class="list font-24 color2 d-flex ">
				<view class="one bg-f5 text-center">
					<view  v-for="(item,index) of menuData" 
					:key="item.id" :class="menuIndex==index?'on':''" @click="changeMenuIndex(index)">{{item.title}}</view>
				</view>
				<view class="two flex-1 borderT-f5 borderB-f5">
					<view class="d-flex a-center j-sb px-2"  
						:class="Utils.inArray(item.id,menuIdArray)>-1?'on':''" @click="chooseMenuId(index)"
						v-for="(item,index) of menuData[menuIndex].children"
						:key="item.id"
						>
						{{item.title}}
						<image :src="Utils.inArray(item.id,menuIdArray)>-1?'../../static/images/i releasel-icon1.png':'../../static/images/i releasel-icon.png'" 
						class="icon3"></image>
					</view>
				</view>
			</view>
			<view class="Rcolor font-22 text-center py-4">如果没有您想要的分类请添加微信客服（微信号：{{kefu}}）</view>
			
			<view class="my-5" @click="submitDataTwo">
				<view class="btn400">{{!isEdit?'确认发布':'确认修改'}}</view>
			</view>
		</view>
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0" v-show="city||saleCity">
			<view  v-show="saleCity">
				<view class="d-flex a-center j-sb px-3 py-2 borderB-e1 bg-white">
					<view class="font-26 Mcolor d-flex a-center" @click="showChoose('saleCity')"><image src="../../static/images/used to releasel-icon4.png" class="icon4"></image>返回</view>
					<view class="bg-mcolor colorf rounded10 yesBtn" @click="confirmSalecity()">确认</view>
				</view>
				
				<view class="classfiy font-26 color2 line-h text-center d-flex">
					<view class="left">
						<view class="li py-3" v-for="(item,index) of cityData" :key="item.id"
						 @click="changeSalecityIndex(index)" :class="salecityIndex==index?'on':''">{{item.title}}</view>
					</view>
					<view class="right flex-1 bg-white">
						<view class="li" :class="Utils.inArray(item.id,salecityIdArray)>-1?'on':''" @click="chooseSalecityId(index)" 
						v-for="(item,index) of cityData[salecityIndex].children"
						:key="item.id">{{item.title}}
							<image src="../../static/images/used to releasel-icon5.png" class="icon5" v-if="Utils.inArray(item.id,salecityIdArray)>-1"></image>
						</view>
					</view>
				</view>
			</view>
			<view  v-show="city">
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
				menuIndex:0,
				cityIndex:0,
				salecityIndex:0,
				navCurr:1,
				classfiy:2,
				submitData:{
					type:2,
					thirdapp_id:2,
					title:'',
					name:'',
					phone:'',
					description:'',
					mainImg:[],
					location:''
				},
				city:false,
				saleCity:false,
				
				cityIdIndex:-1,
				
				Utils:this.$Utils,
				cityData:[],
				salecityIdArray:[],
				salecityTitleArray:[],
				conSalecityTitleArray:[],
				
				menuData:[],
				menuIdArray:[],
				isEdit:false,
				kefu:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMenuData','getCityData'], self);
			self.kefu = uni.getStorageSync('kefu');
			if(options.type){
				self.isEdit = true
				self.getMessageData()
			}
		},
		
		
		
		methods: {
			
			checkPhone() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem ={
					thirdapp_id: 2,
					type:2,
					user_type:0,
					phone:self.submitData.phone
				};
				postData.noShowLoading = true;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						uni.showModal({
							title:'提示',
							content:'你所填写的手机号'+self.submitData.phone+'已被使用，请更换手机号，或者联系平台客服（微信号:'+ self.kefu +'）进行处理',
							showCancel:false,
							confirmText:'我知道了',
							success(res) {
								if(res.confirm){
									self.submitData.phone = ''
								}
							}
						})
					}
				};
				self.$apis.messageGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.mainImg;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入真实有效的手机号', 'none', 1000)
						return;
					}
					//self.messageAdd();
					self.navCurr = 2
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全本页必须信息', 'none')
				};
			},
			
			submitDataTwo(){
				const self = this;
				if(self.isEdit){
					self.messageUpdate()
				}else{
					self.messageAdd();
				}
			},
			
			
			messageUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.menuIdArray.length==0){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('至少选择一项分类', 'none')
					return
				};
				const postData = {};
				postData.searchItem = {
					id:self.messageData.id
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [];
				for (var i = 0; i < self.conSalecityTitleArray.length; i++) {
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:self.conSalecityTitleArray[i],
								thirdapp_id:2,
								type:1
							},
						},
					)	
				};
				if(self.conSalecityTitleArray.length==0){
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:'全国',
								thirdapp_id:2,
								type:1
							},
						},
					)	
				};
				if(self.willDeleteArray.length>0){
					for (var i = 0; i < self.willDeleteArray.length; i++) {
						postData.saveAfter.push(
							{
								tableName: 'Relation',
								FuncName: 'update',
								data: {
									status:-1
								},
								searchItem:{
									id:self.willDeleteArray[i]
								}
							},
						)	
					}
				};
				for (var i = 0; i < self.menuIdArray.length; i++) {
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:self.menuIdArray[i],
								thirdapp_id:2,
								type:2
							},
						},
					)	
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
				if(self.menuIdArray.length==0){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('至少选择一项分类', 'none')
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [];
				for (var i = 0; i < self.conSalecityTitleArray.length; i++) {
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:self.conSalecityTitleArray[i],
								thirdapp_id:2,
								type:1
							},
						},
					)	
				};
				if(self.conSalecityTitleArray.length==0){
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:'全国',
								thirdapp_id:2,
								type:1
							},
						},
					)	
				};
				for (var i = 0; i < self.menuIdArray.length; i++) {
					postData.saveAfter.push(
						{
							tableName: 'Relation',
							FuncName: 'add',
							data: {
								res:{relation_one:'id'},
								relation_two:self.menuIdArray[i],
								thirdapp_id:2,
								type:2
							},
						},
					)	
				};
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
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 3-self.submitData.mainImg.length,
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
			
			changeNav(i){
				const self = this;
				self.navCurr = i
			},
			
			changeClass(i){
				const self = this;
				self.classfiy = i
			},
			
			changeSalecityIndex(index){
				const self = this;
				self.salecityIndex = index
			},
			
			chooseSalecityId(index){
				const self = this;
				var options = self.salecityIdArray.indexOf(self.cityData[self.salecityIndex].children[index].id)
				if(options>-1){
					self.salecityIdArray.splice(parseInt(options),1)
					self.salecityTitleArray.splice(parseInt(options),1)
				}else{
					self.salecityIdArray.push(self.cityData[self.salecityIndex].children[index].id);
					self.salecityTitleArray.push(self.cityData[self.salecityIndex].children[index].title);
				}
			},
			
			
			chooseMenuId(index){
				const self = this;
				
				var options = self.menuIdArray.indexOf(self.menuData[self.menuIndex].children[index].id)
				console.log('options',options)
				if(options>-1){
					self.menuIdArray.splice(parseInt(options),1)
				}else{
					if(self.menuIdArray.length>=5){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('最多选5项', 'none')
						return
					};
					self.menuIdArray.push(self.menuData[self.menuIndex].children[index].id);
				}
			},
			
			confirmSalecity(){
				const self = this;
				self.conSalecityTitleArray = self.salecityTitleArray
				self.saleCity = false;
			},
			
			showChoose(e){
				const self = this;
				self[e] = !self[e];
			},
			
			changeCityIndex(index){
				const self = this;
				self.cityIndex = index
			},
			
			changeMenuIndex(index){
				const self = this;
				self.menuIndex = index
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
			
			getMessageData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type:2,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					relation:{
						tableName:'Relation',
						middleKey:'id',
						key:'relation_one',
						searchItem:{
							status:1,
							//type:2
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData = res.info.data[0];
						self.submitData.title = self.messageData.title;
						self.submitData.name = self.messageData.name;
						self.submitData.phone = self.messageData.phone;
						self.submitData.description = self.messageData.description;
						self.submitData.mainImg = self.messageData.mainImg;
						self.submitData.location = self.messageData.location;
						self.cityIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[0];
						self.cityIdIndex = self.$Utils.findItemInTwoArray(self.cityData,self.submitData.location)[1];
						self.willDeleteArray = [];
						for (var i = 0; i < self.messageData.relation.length; i++) {
							self.willDeleteArray.push(self.messageData.relation[i].id)
							if(self.messageData.relation[i].type == 2){
								self.menuIdArray.push(parseInt(self.messageData.relation[i].relation_two))
							}
							if(self.messageData.relation[i].type == 1&&self.messageData.relation[i].relation_two!='全国'){
								self.salecityTitleArray.push(self.messageData.relation[i].relation_two)
							}
						}
						self.conSalecityTitleArray = self.salecityTitleArray;
						for (var i = 0; i < self.salecityTitleArray.length; i++) {
							self.salecityIdArray.push(self.$Utils.findItemInOfText(self.cityData,self.salecityTitleArray[i]))
						};
						console.log('self.menuIdArray',self.menuIdArray)
					};
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	}
</script>

<style>
.nav .item{width: 50%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 12rpx;height: 22rpx;}
.icon2{width: 19rpx;height: 18rpx;}
textarea{border: 1px solid #e1e1e1; padding: 30rpx;width: 100%;box-sizing: border-box;}


.one{line-height: 80rpx;width: 160rpx;padding-bottom: 160rpx;}
.one .on{background-color: #fff;}
.two{line-height: 80rpx;}
.two view{border-bottom: 1px solid #f5f5f5;}
.icon3{width: 40rpx;height: 40rpx;}

.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}
</style>
