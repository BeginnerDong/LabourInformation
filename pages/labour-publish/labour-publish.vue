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
			<view>添加图片（最多5张图片）</view>
			<view class="uploadImg d-flex a-center">
				<view class="position-relative">
					<image src="../../static/images/second-handl-img2.png" mode=""></image>
					<image src="../../static/images/used to releasel-icon1.png" class="icon2"></image>
				</view>
				<image src="../../static/images/used to releasel-icon.png" mode=""></image>
			</view>
		</view>
		<!-- 列表信息 -->
		<view class="bg-white">
			<view class="py-4 borderB-f5 px-3">
				<view class="d-flex a-center j-sb color2 pb-4 line-h">
					<view class="font-28">标题/招工人需求</view>
					<view class="font-24">(0/100)</view>
				</view>
				<textarea value="" placeholder="请填写100个字以内的内容" />
			</view>
			
			<!-- ---------工人找活展示------- -->
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3">
				<view class="font-28 color2">求职岗位</view>
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon4"></image>
				</view>
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3">
				<view class="font-28 color2">期望薪资</view>
				<input type="text" placeholder="请填写" />
			</view>
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==3 || navCurr==4">
				<view class="font-28 color2">户籍所在地</view>
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon4"></image>
				</view>
			</view>
			<!-- ------------------------------- -->
			
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3" v-show="navCurr==1 || navCurr==2">
				<view class="font-28 color2">工程所在地区</view>     
				<view class="d-flex a-center">
					<view class="font-24 color9 pr-1">请选择</view>
					<image src="../../static/images/used to releasel-icon3.png" class="icon4"></image>
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
		</view>
		
		
		<view class="card1 bg-white">
			<!-- 招工人 -->
			<view class="px-3 mt-2" v-show="navCurr==1">
				<view class="color2 font-28 d-flex a-center">
					<view class="pt-4 pb-3 select">招聘工种</view>
					<view class="pt-4 pb-3 select1">人数</view>
					<view class="pt-4 pb-3 select1">月工资</view>
				</view>
				
				<view class="d-flex a-center pb-2">
					<view class="font-26 position-relative select">
						<input type="text" placeholder="请选择" value="电焊工" class="text-left border-e1 rounded color2"/>
						<view class="position-absolute z-index10 bg-white w-100 border-e1 on" v-show="select_show">
							<view class="borderB-e1">电焊工</view>
							<view>挖机工</view>
						</view>
						<view @click="select()" class="icon2Box">
							<image src="../../static/images/second-handl-icon4.png" class="icon2" v-if="select_show"></image>
							<image src="../../static/images/second-handl-icon.png" class="icon2" v-else></image>
						</view>
					</view>
					<input type="text" placeholder="请填写" value="3" class="text-left border-e1 rounded color2 select1"/>
					<input type="text" placeholder="不填为面议" class="text-left border-e1 rounded color2 select1"/>
				</view>
				<view class="d-flex a-center pb-2">
					<view class="font-26 position-relative select">
						<input type="text" placeholder="请选择" class="text-left border-e1 rounded color2"/>
						<view class="icon2Box">
							<!-- <image src="../../static/images/second-handl-icon4.png" class="icon2" ></image> -->
							<image src="../../static/images/second-handl-icon.png" class="icon2"></image>
						</view>
					</view>
					<input type="text" placeholder="请填写" class="text-left border-e1 rounded color2 select1"/>
					<input type="text" placeholder="不填为面议" class="text-left border-e1 rounded color2 select1"/>
					<view class="font-28 Rcolor">删除</view>
				</view>
			</view>
			<!-- 找队伍 -->
			<view class="px-3 mt-2" v-show="navCurr==2">
				<view class="color2 font-28 d-flex a-center">
					<view class="pt-4 pb-3 select">招聘工种</view>
					<view class="pt-4 pb-3 select1">队伍数量</view>
					<view class="pt-4 pb-3 select1">待遇</view>
				</view>
				<view class="d-flex a-center pb-2">
					<view class="font-26 position-relative select">
						<input type="text" placeholder="请选择" class="text-left border-e1 rounded color2"/>
						<view class="icon2Box">
							<!-- <image src="../../static/images/second-handl-icon4.png" class="icon2" ></image> -->
							<image src="../../static/images/second-handl-icon.png" class="icon2"></image>
						</view>
					</view>
					<input type="text" placeholder="请填写" class="text-left border-e1 rounded color2 select1"/>
					<input type="text" placeholder="不填为面议" class="text-left border-e1 rounded color2 select1"/>
					<!-- <view class="font-28 Rcolor">删除</view> -->
				</view>
			</view>
			
			<view class="d-flex a-center j-center pb-4 pt-2">
				<image src="../../static/images/labor%20releasel-icon.png" class="icon3"></image>
				<view class="pl-2 font-26 Mcolor">继续添加</view>
			</view>
		</view>
		
		
		<!-- 介绍费 -->
		<view class="mt-2 bg-white pb-5">
			<view class="d-flex a-center j-sb py-4 borderB-f5 px-3">
				<view class="font-28 color2">手机号</view>
				<input type="text" placeholder="请输入金额,可不填写" />
			</view>
			<view class="d-flex p-3 mb-5">
				<view class="font-22 Rcolor">设置介绍说明：</view>
				<view class="font-22 color6 line-h-md">
					<view><text class="number">1</text>你可以设置介绍费，促进他人帮你介绍</view>
					<view><text class="number">2</text>他人帮您介绍成功</view>
					<view><text class="number">3</text>您直接支付介绍着的介绍费(介绍费不用交给平台)</view>
				</view>
			</view>
			<view class="btn400">确认发布</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tips_show:true,
				navCurr:1,
				select_show:false
			}
		},
		created(){
			
		},
		methods: {
			changeTips(){
				const self = this;
				self.tips_show = false
			},
			changeNav(i){
				const self = this;
				self.navCurr = i;
			},
			select(){
				const self = this;
				self.select_show = !self.select_show 
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
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
.select1{width: 160rpx;line-height: 60rpx;text-indent: 10rpx;margin-right: 30rpx;}
.card1 .on{border: 1px solid #51A9E9;}

.icon3{width: 22rpx;height: 22rpx;}
.icon4{width: 12rpx;height: 22rpx;}
.number{border: 1px solid #666;color: #666;border-radius: 50%;width: 24rpx;display: inline-block;line-height: 1;text-align: center;margin-right: 10rpx;}
</style>
