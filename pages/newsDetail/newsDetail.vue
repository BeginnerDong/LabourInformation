<template>
	<view class="px-3">
		<view class="font-40 color2 pt-4 font-w">{{mainData.title}}</view>

		<view class="d-flex a-center j-sb pb-1">
			<view class="d-flex a-center">
				<view class="font-26 color6 pr-3">{{mainData.create_time}}</view>
				<view class="d-flex a-center">
					<image src="../../static/images/detailsl-icon.png" class="icon1"></image>
					<view class="font-24 color6 pl-1 py-3">{{mainData.view_count}}</view>
				</view>
			</view>
			<view class="d-flex a-center">
				<view class="d-flex a-center" @click="Utils.stopMultiClick(collect)">
					<!-- <image src="../../static/images/detailsl-icon1.png" class="icon2"></image> -->
					<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/detailsl-icon1.png':'../../static/images/detailsl-icon5.png'"
					 class="icon2"></image>
					<view class="pl-1">{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'未收藏'}}</view>
				</view>
				<button class="d-flex a-center ml-5" open-type="share">
					<image src="../../static/images/detailsl-icon2.png" class="icon2"></image>
					<view class="pl-1">分享</view>
				</button>
			</view>
		</view>

		<view class="font-30 color6 bg-f5 px-1 d-flex a-center rounded10">
			<image src="../../static/images/home-icon2.png" class="icon3"></image>
			<view class="pl-1 py-2">文章摘要</view>
		</view>

		<view class="font-28 color2 py-5">
			<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchItem: {
					thirdapp_id: 2
				},
				mainData: {},
				Utils: this.$Utils,
			}
		},

		onLoad(options) {
			const self = this;
			if (options.id) {
				self.searchItem.id = options.id
			} else {
				self.$Utils.showToast('数据有误，请重试', 'none');
				setTimeout(function() {
					uni.navigateBack({
						delta: 1
					})
				}, 1000);
			};
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title:self.mainData.title,
					path: '/pages/newsDetail/newsDetail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
				}
			}else{
				return {
					title:self.mainData.title,
					path: '/pages/newsDetail/newsDetail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
				}
			}
		},
		

		methods: {

			collect() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},

			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '收藏成功',
					relation_id: self.mainData.id,
					relation_table: 'Article',
					user_no: uni.getStorageSync('user_info').user_no,
					behavior:1
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.logAdd(postData, callback);
			},


			updateGoodLog() {
				const self = this;

				const postData = {
					searchItem: {
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Article',
							behavior:1,
							type:1
						},
						condition: '='
					}
				};
				postData.saveFunction = [{
					FuncName: 'viewArticle',
					searchItem:{
						id:self.searchItem.id
					}
				}];
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						self.mainData.create_time = self.mainData.create_time.substr(0,10)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		}
	}
</script>

<style>
	.bg-f5 {
		background-color: #f5f5f5;
	}

	.icon1 {
		width: 31rpx;
		height: 23rpx;
	}

	.icon2 {
		width: 33rpx;
		height: 34rpx;
	}

	.icon3 {
		width: 38rpx;
		height: 38rpx;
	}

	.img {
		width: 690rpx;
		height: 400rpx;
	}

	button {
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		border-radius: 0;
		font-size: 14px
	}

	button::after {
		border: none;
	}

	.button-hover {
		color: #000000;
		background: none;
	}
</style>
