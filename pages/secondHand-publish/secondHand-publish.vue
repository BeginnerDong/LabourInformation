<template>
	<view>
		<view class="text-center font-24 color2 line-h p-3 head position-relative" v-show="tips_show">
			<view class="Rcolor pb-3">温馨提示</view>
			<view class="pb-1"><text class="Rcolor">自己发布太麻烦？</text>请添加劳务信息官方微信：hjhsn2500</view>
			<view class="pb-1">我们完全免费帮您发布（平台不收取任何费用）</view>
			<view>劳务信息有强大的资源渠道，能够帮您快速的出售/求购</view>
			<view class="close p-3" @click="changeTips()"><image src="../../static/images/used%20to%20releasel-icon2.png" class="icon1"></image></view>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 shadow-sm bg-white nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeCurr('nav',1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeCurr('nav',2)">出售</view>
		</view>
		
		<!-- 添加图片 -->
		<view class="upload font-24 color2 px-3 py-4 bg-white mb-2">
			<view>添加图片（最多5张图片）</view>
			<view class="uploadImg d-flex a-center">
				<view class="position-relative">
					<image src="../../static/images/second-handl-img2.png" mode=""></image>
					<image src="../../static/images/used to releasel-icon1.png" class="icon2"></image>
				</view>
				<image src="../../static/images/used to releasel-icon.png" mode=""></image>
			</view>
		</view>
		
		<view class="bg-white">
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">分类</view>
				<view class="d-flex a-center" @click="changeCurr('oh',1)">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon3"></image>
				</view>
			</view>
			<view class="py-4 borderB-e1 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28">标题/求购描述</view>
					<view class="font-24">(0/100)</view>
				</view>
				<textarea value="" placeholder="请填写100个字以内的内容" />
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">出售价格</view>
				<view class="font-24 color9">此项不填为面议</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">所在地区</view>
				<view class="d-flex a-center" @click="changeCurr('oh',2)">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon3"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">联系人</view>
				<input type="text" placeholder="请填写" />
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-e1 px-3">
				<view class="font-28 color2">手机号</view>
				<input type="text" placeholder="请填写" />
			</view>
		</view>
		
		<view class="bg-white py-5" @click="Router.redirectTo({route:{path:'/pages/publish-success/publish-success'}})">
			<view class="btn400">确认发布</view>
		</view>
		
		
		
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0" v-show="ohCurr!=0">
			<view class="d-flex a-center j-sb px-3 py-2 borderB-e1 bg-white">
				<view class="font-26 Mcolor d-flex a-center" @click="changeCurr('oh',0)"><image src="../../static/images/used to releasel-icon4.png" class="icon4"></image>返回</view>
				<view class="bg-mcolor colorf rounded10 yesBtn" @click="changeCurr('oh',0)">确认</view>
			</view>
			<!-- 分类 -->
			<view v-show="ohCurr==1">
				<view class="Rcolor px-3 py-2 bg-white font-22 text-center">没有您要的分类请联系客服进行增加分类（微信号：njshd55886）</view>
				<view class="classfiy font-26 color2 line-h text-center d-flex">
					<view class="left">
						<view class="li py-3" @click="changeCurr('classfiy',1)" :class="classCurr==1?'on':''">隧道</view>
						<view class="li py-3" @click="changeCurr('classfiy',2)" :class="classCurr==2?'on':''">机械手/司机</view>
						<view class="li py-3" @click="changeCurr('classfiy',3)" :class="classCurr==3?'on':''">桥梁</view>
						<view class="li py-3" @click="changeCurr('classfiy',4)" :class="classCurr==4?'on':''">路基</view>
					</view>
					<view class="right flex-1 bg-white">
						<view class="li on">挖掘机司机<image src="../../static/images/used to releasel-icon5.png" class="icon5"></image></view>
						<view class="li">推土机司机</view>
						<view class="li">装载机司机</view>
						<view class="li">出渣机司机</view>
					</view>
				</view>
			</view>
			
			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="ohCurr==2">
				<view class="left">
					<view class="li py-3" @click="changeCurr('address',1)" :class="addCurr==1?'on':''">福建</view>
					<view class="li py-3" @click="changeCurr('address',2)" :class="addCurr==2?'on':''">陕西</view>
					<view class="li py-3" @click="changeCurr('address',3)" :class="addCurr==3?'on':''">贵州</view>
					<view class="li py-3" @click="changeCurr('address',4)" :class="addCurr==4?'on':''">上海</view>
				</view>
				<view class="right flex-1 bg-white">
					<view class="li on">西安<image src="../../static/images/used to releasel-icon5.png" class="icon5"></image></view>
					<view class="li">咸阳</view>
					<view class="li">宝鸡</view>
					<view class="li">汉中</view>
					<view class="li">安康</view>
					<view class="li">渭南</view>
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
				navCurr:1,
				ohCurr:0,
				classCurr:1,
				addCurr:1
			}
		},
		methods: {
			changeTips(){
				const self = this;
				self.tips_show = false
			},
			changeCurr(type,i){
				const self = this;
				if(type == 'nav'){
					self.navCurr = i;
				}else if(type == 'classfiy'){
					self.classCurr = i;
				}else if(type == 'address'){
					self.addCurr = i;
				}else if(type == 'oh'){
					self.ohCurr = i;
				}
			}
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
