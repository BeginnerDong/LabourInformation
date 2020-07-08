<template>
	<view>
		<view class="head px-3 py-2 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar+23 +'px'}" @click="Router.redirectTo({route:{path:'/pages/labour-publish/labour-publish'}})">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		
		<!-- 搜索 -->
		<view class="bg-f5 px-3 pt-2">
			<view class="border-e1 rounded font-28 px-3 py-2 d-flex a-center j-sb ss">
				<input type="text" value="" />
				<view class="px-2">搜索</view>
			</view>
		</view>
		
		<view class="iconBox d-flex a-center j-sb px-3 pt-4 bg-f5">
			<image src="../../static/images/laborl-icon.png" mode=""></image>
			<image src="../../static/images/laborl-icon1.png" mode=""></image>
			<image src="../../static/images/laborl-icon2.png" mode=""></image>
			<image src="../../static/images/laborl-icon3.png" mode=""></image>
			<image src="../../static/images/laborl-icon4.png" mode=""></image>
		</view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">默认</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">介绍费</view>
			<view class="item d-flex a-center j-center" :class="navCurr==3?'on':''" @click="changeNav(3)">
				所在地
				<image src="../../static/images/second-handl-icon4.png" v-if="navCurr==3"></image>
				<image src="../../static/images/second-handl-icon.png" v-else></image>
			</view>
		</view>
		
		<view class="bg-f5">
			<view class="bg-white px-3" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
				<view class="font-30 color2 pt-4">隧道施工队继续开挖班，处置办，洞1千米长，三级围岩，有经验的队伍请来咨询</view>
				<view class="d-flex j-sb flex-wrap py-3 imgBox">
					<image src="../../static/images/laborl-img.png" mode=""></image>
					<image src="../../static/images/laborl-img.png" mode=""></image>
					<image src="../../static/images/laborl-img.png" mode=""></image>
					<image src="../../static/images/laborl-img.png" mode=""></image>
				</view>
				<view class="d-flex a-center j-sb pb-3">
					<view>
						<view class="tag tagB">招队伍</view>
						<view class="tag tagY">信息已失效</view>
					</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon3.png" class="icon2"></image>
						<view class="font-24 color6 pl-1">西安</view>
						<view class="font-24 color6 pl-2">4-23</view>
					</view>
				</view>
				<view class="line-h-md py-4 font-24 color6 dashedBorder">
					<view>开挖班，1支队伍，待遇面议</view>
					<view>初支班，1支队伍，待遇面议</view>
				</view>
			</view>
			
			<view class="mt-2 bg-white px-3" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
				<view class="font-30 color2 pt-4">隧道施工队继续开挖班，处置办，洞1千米长，三级围岩，有经验的队伍请来咨询</view>
				<view class="d-flex a-center j-sb py-3">
					<view class="d-flex a-center">
						<view class="tag tagG">工人找活</view>
						<view><text class="tag tagR">介绍费</text><text class="tag tagO">1235</text></view>
					</view>
					<view class="d-flex a-center">
						<image src="../../static/images/detailsl-icon3.png" class="icon2"></image>
						<view class="font-24 color6 pl-1">西安</view>
						<view class="font-24 color6 pl-2">2020-06-25</view>
					</view>
				</view>
				<view class="line-h-md py-4 font-24 color6 dashedBorder">
					<view>求职岗位：装载机，期待工资7500</view>
				</view>
			</view>
			
			<view class="py-5 font-26 color9 text-center">没有更多内容了</view>
		</view>
		
		
		<!-- 未搜索到信息 -->
<!-- 		<view class="my-5 font-26 color2 line-h text-center">
			<view class="pb-3">没有搜索到与“装载机”相关的信息</view>
			<view>请试试其他关键词</view>
		</view> -->
		
		
		
		
		
		<view style="height: 100rpx;width: 100%;"></view> 
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
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/business/business'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>商务通</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/labour/labour'}})">
				<image src="../../static/images/nabar4-a.png" mode=""></image>
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
				navCurr:1
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i;
			}
		}
	}
</script>

<style>
.bg-f5{background-color: #f5f5f5;}
.head{position: fixed;top: 0;left: 0;right: 0;}
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}

.ss{margin-top: 145rpx;}
.ss input{border-right: 1px solid #e1e1e1;flex: 1;text-indent: 20rpx;}
.ss input::-webkit-input-placeholder{color: #222!important;}

.iconBox image{width: 110rpx;height: 110rpx ;}
.nav image{width: 13rpx;height: 6rpx;margin-left: 8rpx;}
.nav .item{width: 33.33%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.imgBox image{width: 160rpx;height: 160rpx;}
.icon2{width: 21rpx;height: 25rpx;}

.tagR{margin-right: 0;}

.dashedBorder{border-top: 1px dashed #e1e1e1;}
</style>
