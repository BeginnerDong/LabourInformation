<template>
	<view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">咨询</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">二手信息</view>
			<view class="item" :class="navCurr==3?'on':''" @click="changeNav(3)">招聘信息</view>
			<view class="item" :class="navCurr==4?'on':''" @click="changeNav(4)">商务通</view>
		</view>
		
		<view class="font-30 color2 p-3 d-flex a-center j-sb borderB-e1">
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
		<view class="list" v-show="navCurr==2">
			<view class="d-flex a-center px-3 pb-3 bg-white">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="item d-flex a-center j-sb py-3 pl-2 bg-white flex-1" @click="Router.redirectTo({route:{path:'/pages/secondHand-detail/secondHand-detail'}})">
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
			</view>
		</view>
		
		<!-- 招聘信息 -->
		<view v-show="navCurr==3" class="bg-white">
			<view class="d-flex a-center px-3">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="pl-2 flex-1" @click="Router.redirectTo({route:{path:'/pages/labour-detail/labour-detail'}})">
					<view class="font-30 color2 pt-4">隧道施工队继续开挖班，处置办，洞1千米长，三级围岩，有经验的队伍请来咨询</view>
					<view class="d-flex flex-wrap py-3 imgBox">
						<image src="../../static/images/laborl-img.png" mode=""></image>
						<image src="../../static/images/laborl-img.png" mode=""></image>
						<image src="../../static/images/laborl-img.png" mode=""></image>
						<image src="../../static/images/laborl-img.png" mode=""></image>
					</view>
					<view class="d-flex a-center j-sb">
						<view>
							<view class="tag tagB">招队伍</view>
							<view class="tag tagY">信息已失效</view>
						</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
							<view class="font-24 color6 pl-1">西安</view>
							<view class="font-24 color6 pl-2">4-23</view>
						</view>
					</view>
					<view class="line-h-md py-4 font-24 color6 dashedBorder">
						<view>开挖班，1支队伍，待遇面议</view>
						<view>初支班，1支队伍，待遇面议</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 商务通 -->
		<view v-show="navCurr==4" class="bg-white">
			<view  class="d-flex a-center px-3">
				<image src="../../static/images/i releasel-icon.png" class="icon5"></image>
				<!-- <image src="../../static/images/i releasel-icon1.png" class="icon5"></image> -->
				<view class="flex-1 ml-2">
					<view class="Mcolor font-30 py-3">贵阳**物资有限公司</view>
					<view class="d-flex a-center j-sb line-h pb-3 borderB-e1">
						<view><text class="borderR-e1 pr-2 mr-2">张经理</text>15868472893</view>
						<view class="d-flex a-center">
							<image src="../../static/images/detailsl-icon3.png" class="icon4"></image>
							<view class="font-24 color6 pl-1">西安</view>
						</view>
					</view>
					<view class="d-flex font-24 pt-3">
						<view class="color6">业务范围：</view>
						<view class="color2 flex-1">
							<view>隧道防水板、爬焊机、爬焊机配件、电焊机、热熔机、LED灯等桥隧物资、代理小挖机、柳工装载机</view>
							<view class="d-flex a-center flex-wrap">
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
						<view class="btn Mborder rounded ml-5">拨打电话</view>
					</view>
					<view class="font-22 color2 py-3">不实信息投诉</view>
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
				navCurr:1
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i
			},
			showToast(){
				const self = this;
				uni.showModal({
					title:'提示',
					content:'确定删除该条信息？'
				})
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.nav .item{width: 25%;line-height: 90rpx;text-align: center;}
.nav .on{position: relative;color: #51A9E9;}
.nav .on::before{content: ''; width: 100%;height: 2rpx;background-color: #51A9E9;position: absolute; bottom: 0;left: 0;}

.icon1{width: 30rpx!important;height: 22rpx!important;}
/* .tjTit{width: 480rpx;} */
.tjImg{width: 210rpx;height: 160rpx;}
.icon2{width: 27rpx!important;height: 27rpx!important;}
.icon4{width: 21rpx;height: 25rpx;}
.icon5{width: 40rpx;height: 40rpx;}
.indexBox2 image{width: 210rpx;height: 160rpx;}
.video{height: 340rpx;width: 100%;}

.list .item image{width: 180rpx;height: 180rpx;}
.list .itemCon .tit{/* width: 480rpx; */line-height: 1.2;}
.list .itemConL{width: 180rpx;height: 180rpx;line-height: 180rpx;background-color: #9DD6FF;}

.imgBox image{width: 145rpx;height: 145rpx;margin-right: 19rpx;}
.imgBox image:nth-child(4n){margin-right: 0;}

.btn{width: 120rpx;line-height: 40rpx;}
.img{width: 106rpx;height: 106rpx;margin-top: 20rpx;margin-right: 20rpx;}
.img:nth-child(4n){margin-right: 0;}
</style>
