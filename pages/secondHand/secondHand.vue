<template>
	<view>
		<view class="head p-3 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar+23 +'px'}" @click="Router.redirectTo({route:{path:'/pages/secondHand-publish/secondHand-publish'}})">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		
		<!-- 搜索 -->
		<view class="border-e1 rounded bg-white font-28 mx-3 py-2 d-flex a-center j-sb z-index1000 ss">
			<input type="text" placeholder="风管" />
			<view class="px-2">搜索</view>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 z-index1000 nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeCurr('nav',1)">全部</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeCurr('nav',2)">出售</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeCurr('nav',3)">求购</view>
			<view class="item d-flex a-center j-center" :class="navCurr==4?'on':''" @click="changeCurr('nav',4)">分类
				<image src="../../static/images/labor releasel-icon1.png" v-if="navCurr==4"></image>
				<image src="../../static/images/labor releasel-icon2.png" v-else></image>
			</view>
			<view class="item d-flex a-center j-center" :class="navCurr==5?'on':''" @click="changeCurr('nav',5)">所在地
				<image src="../../static/images/labor releasel-icon1.png" v-if="navCurr==5"></image>
				<image src="../../static/images/labor releasel-icon2.png" v-else></image>
			</view>
		</view>
		
		<!-- 列表 -->
		<view class="list">
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<image src="../../static/images/second-handl-img1.png" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>贵阳**有限公司 <view class="tag tagName">已实名认证</view></view>
						<view>贵阳</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagB">出售</view>
							<view class="tag tagO">湿喷机</view>
							<view class="tag tagR">置顶</view>
						</view>
						<view class="font-22 color9">1小时前</view>
					</view>
				</view>
			</view>
			
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<image src="../../static/images/second-handl-img.png" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">需要一台220挖机，有合适的记得联系我</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>林先生<view class="tag tagName">已实名认证</view></view>
						<view>遵义</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagG">求购</view>
							<view class="tag tagO">湿喷机</view>
						</view>
						<view class="font-22 color9">昨天</view>
					</view>
				</view>
			</view>
			
			<view class="item d-flex a-center j-sb p-3 bg-white mb-2" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
				<image src="../../static/images/second-handl-img1.png" mode=""></image>
				<view class="itemCon flex-1 ml-2">
					<view class="color3 font-30 avoidOverflow2 tit">隧道佳乐湿喷机2017年的信号HSC-2513，只用了一个工地，机子还是很不错的</view>
					<view class="font-24 color6 d-flex a-center j-sb mt-3 line-h">
						<view>林先生<view class="tag tagName">已实名认证</view></view>
						<view>昆明</view>
					</view>
					<view class="d-flex a-center j-sb h-100 mt-3">
						<view class="font-22 d-flex a-center">
							<view class="tag tagB">出售</view>
							<view class="tag tagO">湿喷机</view>
							<view class="tag tagY">信息已失效</view>
						</view>
						<view class="font-22 color9">7-23</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="oh bg-mask position-fixed top-0 left-0 right-0" :style="{marginTop:statusBar+155 +'px'}" v-show="navCurr==4 || navCurr==5" @click="changeCurr('oh',0)">
			<!-- 分类 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="navCurr==4">
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
			
			<!-- 所在地 -->
			<view class="classfiy font-26 color2 line-h text-center d-flex" v-show="navCurr==5">
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
		
		
		
		<view class="py-5 font-26 color9 text-center">没有更多内容了</view>
		<view style="height: 100rpx;width: 100%;"></view>
		<!-- 底部 -->
		<view class="footer z-index100"> 
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/secondHand/secondHand'}})">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
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
	export default {
		data() {
			return {
				Router:this.$Router,
				statusBar: app.globalData.statusBar,
				navCurr:1,
				classCurr:1,
				addCurr:1,
				ohCurr:0
			}
		},
		methods: {
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
.head{position: fixed;top: 0;left: 0;right: 0;}
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}

.ss{margin-top: 175rpx;}
.ss input{border-right: 1px solid #e1e1e1;flex: 1;padding-left: 20rpx;font-size: 28rpx;text-align: left;box-sizing: border-box;}
.ss input::-webkit-input-placeholder{color: #222!important;}

.nav image{width: 18rpx;height: 9rpx;margin-left: 8rpx;}
.nav .item{width: 20%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.list .item image{width: 180rpx;height: 180rpx;}
.list .itemCon .tit{width: 480rpx;line-height: 1.2;}
.list .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.icon4{width: 14rpx;height: 26rpx;margin-right: 10rpx;}
.yesBtn{padding: 15rpx 30rpx;}
.classfiy .left{width: 180rpx;background-color: #f5f5f5;}
.classfiy .left .on{color: #55AAE9;position: relative;background: #fff;}
.classfiy .left .on::before{content: '';background-color: #55AAE9;height: 100%;width: 4rpx;position: absolute;top: 0;left: 0;}
.icon5{width: 36rpx;height: 26rpx;}
.classfiy .right .li{display: flex;justify-content: space-between;align-items: center;padding: 30rpx;border-bottom: 1px solid #f5f5f5;}
.classfiy .right .on{color: #51A9E9;}
</style>
