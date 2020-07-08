<template>
	<view class="h-100">
		<view class="head p-3 bg-mcolor font-36 colorf d-flex a-center z-index100" :style="{paddingTop:statusBar+23 +'px'}" @click="Router.redirectTo({route:{path:'/pages/business-publish/business-publish'}})">
			<image src="../../static/images/businessl-icon1.png" class="icon1"></image>
			<view>免费发布</view>
		</view>
		 <!--  -->
		<view class="d-flex h-100 con" :style="{marginTop:statusBar+64 +'px'}">
			<!-- 左边 -->
			<view class="list font-28 color2 d-flex flex-column a-center j-center">
				<view class="d-flex a-center j-center" :class="listCurr==0?'on':''" @click="changeList(0)"><image src="../../static/images/businessl-icon.png" class="icon2"></image>搜索</view>
				<view :class="listCurr==1?'on':''" @click="changeList(1)">为您推荐</view>
				<view :class="listCurr==2?'on':''" @click="changeList(2)">管棚施工</view>
				<view :class="listCurr==3?'on':''" @click="changeList(3)">隧道机械</view>
				<view :class="listCurr==4?'on':''" @click="changeList(4)">桥梁机械</view>
				<view :class="listCurr==5?'on':''" @click="changeList(5)">物资</view>
				<view :class="listCurr==6?'on':''" @click="changeList(6)">材料</view>
				<view class="flex-1"></view>
			</view>
			<!-- 右边 -->
			<view class="flex-1 bg-white h-100" v-show="listCurr==1">
				<view class="rounded shadow m-2 px-2">
					<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
						<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
							<view class="font-24 color6 pl-1">西安</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">销售区域：</view>
						<view class="color2 flex-1">全国</view>
					</view>
					<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
						<view class="btn Mborder rounded">收藏</view>
						<view class="btn Mborder rounded ml-5">拨打电话</view>
					</view>
					<view class="font-22 color2 py-3">不实信息投诉</view>
				</view>
				
				<view class="rounded shadow m-2 px-2">
					<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
						<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
							<view class="font-24 color6 pl-1">西安</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">
							<view>隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
							<view class="d-flex a-center j-sb flex-wrap">
								<image src="../../static/images/second-handl-img1.png" class="img"></image>
								<image src="../../static/images/second-handl-img1.png" class="img"></image>
								<image src="../../static/images/second-handl-img1.png" class="img"></image>
							</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">销售区域：</view>
						<view class="color2 flex-1">全国</view>
					</view>
					<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
						<view class="btn Mborder rounded">收藏</view>
						<view class="btn Mborder rounded ml-5">拨打电话</view>
					</view>
					<view class="font-22 color2 py-3">不实信息投诉</view>
				</view>
				
			</view>
			
			<!-- 搜索 -->
			<view class="bg-white flex-1 p-2 search" v-show="listCurr==0">
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
					<view class="rounded shadow my-2 px-2">
						<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
						<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
							<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
							<view class="d-flex a-center">
								<image src="../../static/images/detailsl-icon3.png" class="icon3"></image>
								<view class="font-24 color6 pl-1">西安</view>
							</view>
						</view>
						<view class="d-flex font-24 pt-3">
							<view class="color6">业务范围：</view>
							<view class="color2 flex-1">隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
						</view>
						<view class="d-flex font-24 pt-3">
							<view class="color6">销售区域：</view>
							<view class="color2 flex-1">全国</view>
						</view>
						<view class="font-22 Mcolor text-center d-flex a-center j-end pt-3">
							<view class="btn Mborder rounded">收藏</view>
							<view class="btn Mborder rounded ml-5">拨打电话</view>
						</view>
						<view class="font-22 color2 py-3">不实信息投诉</view>
					</view>
				</view>
			</view>
			
		</view>
		
		
		
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
				listCurr:0
			}
		},
		methods: {
			changeList(i){
				const self = this;
				self.listCurr = i
			}
		}
	}
</script>

<style>
page{background-color: #D8EDFE;height: 100%;}
.bg-f5{background-color: #f5f5f5;}
.head{position: fixed;top: 0;left: 0;right: 0;}
.icon1{width: 23rpx;height: 23rpx;margin-right: 10rpx;}
.icon2{width: 24rpx;height: 24rpx;margin-right: 10rpx;}
.icon3{width: 21rpx;height: 25rpx;}
.img{width: 106rpx;height: 106rpx;margin-top: 20rpx;}

/* .listBox{width: 180rpx;} */
.list{width: 180rpx;line-height: 100rpx;text-align: center;top: 0;height: 100%;}
.list view{width: 100%;}
.list .on{background-color: #fff;}

.btn{width: 120rpx;line-height: 40rpx;}

.shadow{box-shadow: 0 0rpx 16rpx rgba(114, 130, 138, 0.2)!important;}

.con{overflow-y: scroll;}

.search input{text-align: left;color: #222;font-size: 28rpx;flex: 1;}
.search .ss{height: 80rpx;}
.w3{width: 33.33%;}
</style>
