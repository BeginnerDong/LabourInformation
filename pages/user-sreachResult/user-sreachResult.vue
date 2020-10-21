<template>
	<view>
		
		<view class="font-24 color2 p-3 head position-relative">
			<text class="Rcolor">特别声明：</text>
			本次查询的内容均为网友提交，不能代表信息的真实性，仅供参考，如有涉及不实信息请及时与我们联系，我们将第一时间进行删除
			<text class="Mcolor" style="float: right;" @click="Router.navigateTo({route:{path:'/pages/user-opinion/user-opinion'}})">举报不实信息</text>
		</view>
		<view style="height: 20rpx;" class="bg-f5"></view>
		<view class="font-28 color2 py-3 text-center">
			本次共查询{{num}}条信息，其中<text class="Rcolor">{{mainData.length?mainData.length:0}}</text>条涉及不良行为
		</view>
		<view style="height: 20rpx;" class="bg-f5"></view>
		
		<!-- 结果 -->
		<view>
			<view class="font-26 text-center color9 pt-5 mt-3" v-if="mainData.length==0">未查询到不良结果</view>
			
			<view class="font-26 color2 borderB-e1" v-if="mainData.length>0">
				<view class="borderB-f5 p-3" v-for="(item,index) in mainData" :key="index">
					<view class="pb-3">姓名：{{item.title!=''?item.title:'无'}}</view>
					<view class="pb-3">手机号：{{item.phone!=''?item.phone:'无'}}</view>
					<view class="pb-3">身份证号：{{item.description!=''?item.description:'无'}}</view>
					<view class="d-flex">
						<view>不良记录：</view>
						<view class="flex-1 line-h-md">{{item.content}}</view>
					</view>
					<view class="d-flex"  v-if="item.mainImg.length>0">
						<view class="pt-2">相关图片：</view>
						<view class="flex-1 d-flex flex-wrap">
							<!-- <image src="../../static/images/laborl-img.png"  class="img"></image> -->
							<image @click="imgPreview(index,c_index)" v-for="(c_item,c_index) of item.mainImg" :key="c_index" :src="c_item.url" class="img"></image>
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
				mainData:[],
				num:0
			}
		},
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('infoData');
			self.num = uni.getStorageSync('infoNum')
		},
		methods: {
			imgPreview(index,c_index){
				const self = this;
				var urls = [];
				for (var i = 0; i < self.mainData[index].mainImg.length; i++) {
					urls.push(self.mainData[index].mainImg[i].url)
				};
				uni.previewImage({
					current:c_index,
					urls:urls,
				})
			},
		}
	}
</script>

<style>
.head{background-color: #FFF4D4;}
.img{width: 140rpx;height: 140rpx;margin-right: 20rpx;margin-top: 20rpx;}
</style>
