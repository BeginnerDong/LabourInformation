<template>
	<view>
		
		<!-- nav -->
		<view class="font-28 color2 d-flex a-center j-sb borderB-e1 bg-f5 shadow-sm nav">
			<view class="item" :class="navCurr==1?'on':''" @click="changeNav(1)">基本信息</view>
			<view class="item" :class="navCurr==2?'on':''" @click="changeNav(2)">名片类型</view>
		</view>
		
		<!-- 基本信息 -->
		<view v-show="navCurr==1" class="pb-5">
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==1 || navCurr==2">
				<view class="font-28 color2">公司名称</view>     
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==1 || navCurr==2">
				<view class="font-28 color2">所在地区</view>     
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">联系人</view>
				<input type="text" placeholder="请填写" />
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">手机号</view>
				<input type="text" placeholder="请填写" />
			</view>
			
			<view class="py-4 borderB-f5 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28">业务范围</view>
					<view class="font-24">(0/80)</view>
				</view>
				<textarea value="" placeholder="请填写800个字以内的内容" />
			</view>
			
			<!-- 添加图片 -->
			<view class="upload font-24 color2 px-3 py-4 bg-white borderB-f5">
				<view>添加图片（最多5张图片）</view>
				<view class="uploadImg d-flex a-center">
					<view class="position-relative">
						<image src="../../static/images/second-handl-img2.png" mode=""></image>
						<image src="../../static/images/used to releasel-icon1.png" class="icon2"></image>
					</view>
					<image src="../../static/images/used to releasel-icon.png" mode=""></image>
				</view>
			</view>
			
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==1 || navCurr==2">
				<view class="font-28 color2">销售区域（可多选</view>     
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">不填写默认全国</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon1"></image>
				</view>
			</view>
			
			<view class="my-5">
				<view class="btn400">下一步</view>
			</view>
		</view>
		
		<!-- 名片类型 -->
		<view class="" v-show="navCurr==2">
			<view class="font-24 color2 font-w text-center py-4">请选择名片分类（最多选择5个）</view>
			<view class="list font-24 color2 d-flex ">
				<view class="one bg-f5 text-center">
					<view :class="classfiy==1?'on':''" @click="changeClass(1)">一级分类</view>
					<view :class="classfiy==2?'on':''" @click="changeClass(2)">一级分类</view>
					<view :class="classfiy==3?'on':''" @click="changeClass(3)">一级分类</view>
					<view :class="classfiy==4?'on':''" @click="changeClass(4)">一级分类</view>
				</view>
				<view class="two flex-1 borderT-f5 borderB-f5">
					<view class="d-flex a-center j-sb px-2">二级分类<image src="../../static/images/i releasel-icon1.png" class="icon3"></image></view>
					<view class="d-flex a-center j-sb px-2">二级分类<image src="../../static/images/i releasel-icon.png" class="icon3"></image></view>
					<view class="d-flex a-center j-sb px-2">二级分类<image src="../../static/images/i releasel-icon.png" class="icon3"></image></view>
					<view class="d-flex a-center j-sb px-2">二级分类<image src="../../static/images/i releasel-icon.png" class="icon3"></image></view>
				</view>
			</view>
			<view class="Rcolor font-22 text-center py-4">如果没有您想要的分类请添加微信客服（微信号：zdv223568）</view>
			
			<view class="my-5">
				<view class="btn400">确认发布</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:1,
				classfiy:2
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i
			},
			changeClass(i){
				const self = this;
				self.classfiy = i
			}
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
</style>
