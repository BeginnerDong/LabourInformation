<template>
	<view>

		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-3">分享到朋友圈步骤</view>
			<view><text>1</text>先保存图片><text>2</text>将图片发送到朋友圈</view>
			<view>他人可通过点开图片长按识别小程序码，查看本条信息</view>
		</view>
		<view style="height: 20rpx;width: 100%;background-color: #f5f5f5;"></view>
		<view class="pc-container" style="position: absolute;top: 0;z-index: -999;">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas" style="width: 690rpx;height: 800rpx;" v-show="canvasShow"></canvas>
		</view>

	


		<!-- 出售求购分享 -->
		<view class="bg-white  px-3">
			<view class="py-3 font-32 color2 text-center">图片样式</view>
			<view class="rounded10 overflow-h shadow pb-3">
				<view class="d-flex flex-wrap pr-1">
					<image src="../../static/images/second-handl-img2.png" class="img1"></image>
					<view class="p-2 color6 w-50">
					{{mainData.title?mainData.title:''}}</view>
					<view class="w-50 pt-3">
						<image :src="qrUrl" class="img2"></image>
						<view class="font-22 color6 px-5 text-center">长按识别图中小程序码查看详情</view>
					</view>
					<view class="w-50 pt-2">
						<view class="font-32 Mcolor d-flex a-center pb-1">
							<image src="../../static/images/second-handl-icon1.png" class="icon4"></image>{{mainData.city?mainData.city.title:''}}
						</view>
						<view class="font-28 color2 d-flex line-h a-center j-sb borderT-e1 py-2">
							<view class="d-flex a-center">
								<image src="../../static/images/second-handl-icon2.png" class="icon5"></image>{{mainData.name?mainData.name:''}}
							</view>
							<view class="d-flex a-center">
								<image src="../../static/images/second-handl-icon3.png" class="icon6"></image>{{mainData.phone?mainData.phone:''}}
							</view>
						</view>
						<view class="font-22 color6 pt-2 border-e1 txt pl-1 line-h">
							<view class="pb-2">桥隧之家-完全免费的信息平台</view>
							<view class="pb-2">您可以</view>
							<view class="pb-2">1.快速的出售/求购二手机械设备</view>
							<view class="pb-2">2.快速查询劳务相关资源供应商</view>
							<view class="pb-2">3.快速招民工/班组，或者求职</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 text-center pt-3">没有识别出小程序信息，请重新点开图片，多试几次就行了</view>
			</view>

			<view class="py-5" @click="submit">
				<view class="btn400">保存图片</view>
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgurl: '',
				canvasShow: true,
				mainData: {},
				qrUrl:''
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.qrUrl = uni.getStorageSync('secondHandQr');
			self.$Utils.loadAll(['getMainData'], self);

		},

		computed: {

		},

		watch: {

		},


		methods: {
			
			submit(){
				this.canvasImage()
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id
				};
				postData.getAfter = {
					user: {
						tableName: 'User',
						middleKey: 'user_no',
						key: 'user_no',
						searchItem: {
							status: 1
						},
						condition: '=',
						//info:['headImgUrl']
					},
					label: {
						tableName: 'Label',
						middleKey: 'menu_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
						info: ['title']
					},
					city: {
						tableName: 'Label',
						middleKey: 'location',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
						info: ['title']
					},
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Message'
						},
						condition: '='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						//this.canvasImage();
						//self.mainData.invalid_time = self.$Utils.timeto(self.mainData.invalid_time*1000,'ymd')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},

			//多行文字
			writeTextOnCanvas(ctx_2d, lineheight, bytelength, text, startleft, starttop) {
				function getTrueLength(str) { //获取字符串的真实长度（字节长度）
					var len = str.length,
						truelen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							truelen += 2;
						} else {
							truelen += 1;
						}
					}
					return truelen;
				}

				function cutString(str, leng) { //按字节长度截取字符串，返回substr截取位置
					var len = str.length,
						tlen = len,
						nlen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							if (nlen + 2 < leng) {
								nlen += 2;
							} else {
								tlen = x;
								break;
							}
						} else {
							if (nlen + 1 < leng) {
								nlen += 1;
							} else {
								tlen = x;
								break;
							}
						}
					}
					return tlen;
				}
				for (var i = 1; getTrueLength(text) > 0; i++) {
					var tl = cutString(text, bytelength);
					ctx_2d.fillStyle = '#666';
					ctx_2d.font = '14px Arial';
					ctx_2d.fillText(text.substr(0, tl).replace(/^\s+|\s+$/, ""), startleft, (i - 1) * lineheight + starttop);
					text = text.substr(tl);
				}
			},

			canvasImage() {
				const self = this;
				uni.showLoading({
					title: '正在生成图片',
					mask: true
				})
				let myCanvas = uni.createCanvasContext('mycanvas', this);
				//画布尺寸

				// 坐标(0,0) 表示从此处开始绘制，相当于偏移。
				//主图
				myCanvas.fillStyle = "#FFFFFF";
				myCanvas.fillRect(0, 0, 345, 400);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				if(this.mainData&&this.mainData.mainImg&&this.mainData.mainImg[0]){
					var mainImg = this.mainData.mainImg[0].url;
					wx.downloadFile({
						url: mainImg, //网络路径
						success: function(res) {
							console.log('mainImg',res)
							myCanvas.drawImage(res.tempFilePath, 0, 0, 165, 165);
							myCanvas.save()
							myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
							myCanvas.draw(true)
						}
					});
				};
				
				//myCanvas.drawImage(mainImg, 0, 0, 165, 165);
				//参数：图片，左偏移，上偏移，宽，高
				//二维码
				wx.downloadFile({
					url: this.qrUrl, //网络路径
					success: function(res) {
						console.log('qr',res)
						myCanvas.drawImage(res.tempFilePath, 30, 180, 115, 115);
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					},
					fail(res) {
						console.log('qr',res)
					}
				});
				//myCanvas.drawImage('../../static/images/used to sharel-img.png', 30, 180, 115, 115);
				myCanvas.fillStyle = '#666';
				myCanvas.font = `11px Arial`;
				myCanvas.fillText('长按识别图中小程序码', 30, 310);
				myCanvas.fillText('查看详情', 60, 330);
				//描述
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var content = this.mainData.title;
				this.writeTextOnCanvas(myCanvas, 20, 22, content, 180, 20)
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//所在地
				
				var location = this.mainData.city.title;
				myCanvas.drawImage('../../static/images/second-handl-icon1.png', 175, 175, 11, 13);
				myCanvas.fillStyle = '#51A9E9';
				myCanvas.font = '16px Arial';
				myCanvas.fillText(location, 192, 186);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//下划线
				
				myCanvas.strokeStyle = "#e1e1e1";
				myCanvas.moveTo(175, 195.5);
				myCanvas.lineTo(330, 195.5);
				myCanvas.stroke();
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//联系人信息
				var name = this.mainData.name;
				myCanvas.drawImage('../../static/images/second-handl-icon2.png', 175, 205, 11, 14);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.fillStyle = '#000';
				myCanvas.font = '14px Arial';
				myCanvas.fillText(name, 192, 217);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var phone = this.mainData.phone;
				myCanvas.drawImage('../../static/images/second-handl-icon3.png', 240, 205, 11, 14);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.fillStyle = '#000';
				myCanvas.font = '14px Arial';
				myCanvas.fillText(phone, 253, 217);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//平台介绍	
				myCanvas.strokeRect(175.5, 230.5, 160, 117);
				myCanvas.fillStyle = '#666';
				myCanvas.font = '11px Arial';
				myCanvas.fillText('桥隧之家-完全免费的信息平台', 180, 250);
				myCanvas.fillText('您可以', 180, 270);
				myCanvas.fillText('1.快速的出售/求购二手机械设备', 180, 290);
				myCanvas.fillText('2.快速查询劳务相关资源供应商', 180, 310);
				myCanvas.fillText('3.快速招民工/班组，或者求职', 180, 330);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.fillStyle = '#999';
				myCanvas.fillText('没有识别出小程序信息，请重新点开图片，多试几次就行了', 30, 380);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//开始绘画，必须调用这一步，才会把之前的一些操作实施
				setTimeout(function() {
					uni.canvasToTempFilePath({
						canvasId: 'mycanvas',
						success: (res) => {
							// 在H5平台下，tempFilePath 为 base64
							console.log(res)
							self.imgurl = res.tempFilePath;
							//self.canvasShow = false;
							self.savePoster()
						
							uni.hideLoading();
						
							uni.setStorageSync('person-card', self.imgurl);
						},
						complete(res) {
							console.log(res)
						},
						fail() {
							console.log(res)
						}
					}, this);
				}, 3000,this);
			},

			savePoster() {
				let self = this
				//若二维码未加载完毕，加个动画提高用户体验
				wx.showToast({
					icon: 'loading',
					title: '正在下载图片',
					duration: 1000
				})
				//判断用户是否授权"保存到相册"
				wx.getSetting({
					success(res) {
						//没有权限，发起授权
						if (!res.authSetting['scope.writePhotosAlbum']) {
							wx.authorize({
								scope: 'scope.writePhotosAlbum',
								success() { //用户允许授权，保存图片到相册
									self.savePhoto();
								},
								fail() { //用户点击拒绝授权，跳转到设置页，引导用户授权
									console.log('fail')
									wx.showToast({
										icon: 'none',
										title: '请至小程序设置中开启相册权限',
										duration: 1000
									})
								}
							})
						} else { //用户已授权，保存到相册
							self.savePhoto()
						}
					}
				})
			},

			savePhoto() {
				let self = this
				wx.saveImageToPhotosAlbum({
					filePath: self.imgurl,
					success(res) {
						wx.showToast({
							title: '保存成功',
							icon: "success",
							duration: 1000
						})
						var myCanvas = uni.createCanvasContext('mycanvas', this);
						console.log(myCanvas.width)
						myCanvas.clearRect(0, 0, 345, 400);
						myCanvas.save()
						myCanvas.restore()
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	.pc-container image {
		width: 100%;
		min-height: 800rpx;
		border: 1px solid #BBBBBB;
		display: block;
		border-radius: 10rpx;
	}

	.pc-container {
		width: 690rpx;
		height: 800rpx;
		margin: 0 auto;
		//padding-top: 46rpx;
		overflow: hidden;
	}




	.head text {
		border: 1px solid #D30D11;
		color: #D30D11;
		border-radius: 50%;
		width: 24rpx;
		display: inline-block;
		line-height: 1;
	}

	.icon1 {
		width: 28rpx;
		height: 28rpx;
		margin-right: 20rpx;
	}

	.icon2 {
		width: 30rpx;
		height: 32rpx;
		margin-right: 10rpx;
	}

	.icon3 {
		width: 22rpx;
		height: 30rpx;
		margin-right: 10rpx;
	}

	.icon4 {
		width: 23rpx;
		height: 27rpx;
		margin-right: 10rpx;
	}

	.icon5 {
		width: 22rpx;
		height: 28rpx;
		margin-right: 10rpx;
	}

	.icon6 {
		width: 20rpx;
		height: 28rpx;
		margin-right: 10rpx;
	}

	.shareImg image {
		width: 296rpx;
		height: 296rpx;
	}

	.shareImg image:nth-child(2) {
		width: 323rpx;
		height: 290rpx;
	}

	.img {
		width: 150rpx;
		height: 62rpx;
		margin-left: 10rpx;
	}

	.txt {
		width: 330rpx;
	}

	.img1 {
		width: 330rpx;
		height: 330rpx;
	}

	.img2 {
		width: 230rpx;
		height: 230rpx;
		margin: 0 auto;
	}
</style>
