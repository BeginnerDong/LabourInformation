<template>
	<view>

		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-1 font-30">分享到朋友圈步骤</view>
			<view style="line-height: 50rpx;" class="d-flex a-center j-center">
				<view style="color:red;width: 30rpx;height:30rpx;border-radius: 50%;overflow: hidden;font-size:10px;border: 1px solid red;display: flex;align-items: center;justify-content: center;">1</view>
				<view class="pl-1 pr-1">先保存图片></view>
				<view  style="color:red;width: 30rpx;height:30rpx;border-radius: 50%;overflow: hidden;font-size:10px;border: 1px solid red;display: flex;align-items: center;justify-content: center;">2</view>
				<view class="pl-1 pr-1">将图片发送到朋友圈</view>
			</view>
			<view>他人可通过点开图片长按识别小程序码，查看本条信息</view>
		</view>
		<view style="height: 20rpx;width: 100%;background-color: #f5f5f5;"></view>
		<view class="pc-container" style="position: fixed;left:0;top: 9999px" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas" :style="{width: width+'px',height:height+'px'}"></canvas>
		</view>
		
		<view class="bg-white">
			<view class="py-3 font-32 color2 text-center">保存图片样式</view>
			<view class="rounded10 overflow-h shadow pb-3 wh722" style="margin-left: 14rpx;">
				<!-- 招聘分享 -->
				<view>
					<view class="font-30 color2 d-flex a-center j-sb pr-2 pb-3">
						<view class="font-34 font-w">
							<text class="sgin" v-if="mainData.behavior==1">
								招工人
							</text>
							<text class="sgin" v-if="mainData.behavior==2">
								招队伍
							</text>
							<text class="sgin" v-if="mainData.behavior==3">
								工人找活
							</text>
							<text class="sgin" v-if="mainData.behavior==4">
								队伍找活
							</text>
							
						</view>
						<view>发布日期：{{mainData.create_time?mainData.create_time:''}}</view>
					</view>
<!-- 					<view class="font-32 color2 px-2 line-h-md" style="min-height: 150rpx;">
						{{mainData.title?mainData.title:''}}
					</view>
					 -->
					<view class=" color2 mx-2 line-h-md borderB-f5 pb-3">
						<!-- <view class="font-36 Mcolor font-w text-center pb-2">{{mainData.description?mainData.description:''}}</view> -->
						<view  class="avoidOverflow3 font-34">{{mainData.title?mainData.title:''}}</view>
					</view>
					<!-- <view class="d-flex a-center j-sb mx-2 line-h py-3 borderB-f5 font-24 color2" v-if="mainData.price!=''">
						<view>更多>></view>
						<view class="Rcolor">介绍成功给{{mainData.price?mainData.price:''}}介绍费</view>
					</view> -->
				</view>
				<!-- 找活分享 -->
			

				<view class="font-28 color2 text-center py-3">长按图片，识别图中小程序码，<text class="font-w">免费查看联系方式</text></view>
				<view class="shareImg d-flex j-sb a-center px-3">
					<image :src="qrUrl" mode=""></image>
					<image src="../../static/images/used-to-sharel-img1.png" mode=""></image>
				</view>
				<view class="font-22 color9 text-center pt-3">没有识别出小程序信息，请重新点开图片，多试几次就行了</view>
			</view>

			<!-- <view class="py-5">
				<view class="btn400"  @click="submit">保存图片</view>
			</view> -->
			
			<view class="py-5" v-if="isCom" @click="submit">
				<view class="btn400">保存图片</view>
			</view>
			
			<view class="py-5" v-else>
				<view class="btn400" style="background-color: #999999;">保存图片</view>
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
				qrUrl:'',
				rpx: 0,
				width: 0,
				height: 0,
				isCom:false
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.qrUrl = uni.getStorageSync('labourQr');
			wx.downloadFile({
				url: self.qrUrl, //网络路径
				success: function(res) {
					console.log('qr', res)
					self.isCom = true;
					self.qrCodeUrl = res.tempFilePath
				},
			});
			self.$Utils.loadAll(['getMainData'], self);
			wx.getSystemInfo({
				success: function(res) {
					self.rpx = res.windowWidth / 375;
					self.width = 361 * self.rpx
					self.height = 361 * self.rpx
					
					console.log('self.width', self.width)
				},
			})
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
						self.mainData.create_time = self.mainData.create_time.substr(0,10)
						//this.canvasImage()
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
					ctx_2d.textAlign="left";
					ctx_2d.font = '17px Arial';
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
				//背景色
				myCanvas.fillStyle = "#FFFFFF";
				myCanvas.fillRect(0, 0, 361*self.rpx, 361*self.rpx);
				myCanvas.fillStyle = "#D31114";
				myCanvas.fillRect(0, 0, 80*self.rpx, 30*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//类型
				if(this.mainData.behavior==1){
					var text = '招工人'
				}else if(this.mainData.behavior==2){
					var text = '招队伍'
				}else if(this.mainData.behavior==3){
					var text = '工人找活'
				}else{
					var text = '队伍找活'
				};
				myCanvas.fillStyle = '#fff';
				//myCanvas.font = `17px Arial 600`;
				myCanvas.textAlign="center";
				myCanvas.font = `normal bold ${parseInt(self.rpx * 17)}px sans-serif`;
				myCanvas.fillText(text, 40*self.rpx, 22*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				
				//内容
				myCanvas.fillStyle = '#000000';
				myCanvas.font = `15px Arial`;
				var content = this.mainData.title;
				if(content.length>70){
					this.writeTextOnCanvas(myCanvas, 25*self.rpx, 40*self.rpx, content.substr(0,55)+'...', 10*self.rpx, 60*self.rpx)
				}else{
					this.writeTextOnCanvas(myCanvas, 25*self.rpx, 40*self.rpx, content, 10*self.rpx, 60*self.rpx)
				}
				
				/* var content1 = "";
				var content2 = "";
				var content3 = "";
				myCanvas.font = `16px Arial`;
				myCanvas.fillStyle = '#51A9E9';
				if(this.mainData.behavior!=3){
					if(this.mainData.behavior==1||this.mainData.behavior==2){
						if(this.mainData.passage_array[0]&&this.mainData.passage_array[0].name){
							var money1 = this.mainData.passage_array[0].money==''?'月工资面议':'月工资'+this.mainData.passage_array[0].money
							var content1 = this.mainData.passage_array[0].name + '，' + this.mainData.passage_array[0].num + '队，' + money1
						}
						if(this.mainData.passage_array[1]&&this.mainData.passage_array[1].name){
							var money2 = this.mainData.passage_array[1].money==''?'月工资面议':'月工资'+this.mainData.passage_array[1].money
							var content2 = this.mainData.passage_array[1].name + '，' + this.mainData.passage_array[1].num + '队，' + money2
						}
						if(this.mainData.passage_array[2]&&this.mainData.passage_array[2].name){
							var money3 = this.mainData.passage_array[2].money==''?'月工资面议':'月工资'+this.mainData.passage_array[2].money
							var content3 = this.mainData.passage_array[2].name + '，' + this.mainData.passage_array[2].num + '队，' + money3
						}
					}else if(this.mainData.behavior==4){
						if(this.mainData.passage_array[0]&&this.mainData.passage_array[0].name){
							var content1 = this.mainData.passage_array[0].name
						}
						if(this.mainData.passage_array[1]&&this.mainData.passage_array[1].name){
							var content2 = this.mainData.passage_array[1].name
						}
						if(this.mainData.passage_array[2]&&this.mainData.passage_array[2].name){
							var content3 = this.mainData.passage_array[2].name
						}
					}
					
					myCanvas.fillText(content1, 10, 60);
					myCanvas.fillText(content2, 10, 85);
					myCanvas.fillText(content3, 10, 110);
				}else if(this.mainData.behavior==3){
					myCanvas.textAlign = 'center' //文字居中
					myCanvas.font = 'bold 18px Arial' //文字样式：加粗 16像素 字体Arial
					 myCanvas.fillText(this.mainData.title,175.5,60,345);
				} */
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				
				myCanvas.textAlign="left";
				myCanvas.fillStyle = '#000';
				myCanvas.font = `15px Arial`;
				//发布日期
				var day = '发布日期：'+ this.mainData.create_time.substr(0,10)
				myCanvas.fillText(day, 190*self.rpx, 22*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//二维码
				myCanvas.drawImage(self.qrCodeUrl, 40*self.rpx, 180*self.rpx, 130*self.rpx, 130*self.rpx);
				/* wx.downloadFile({
					url: this.qrUrl, //网络路径
					success: function(res) {
						console.log('qr', res)
						self.isCom = true
						myCanvas.drawImage(res.tempFilePath, 30*self.rpx, 180*self.rpx, 130*self.rpx, 130*self.rpx);
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					},
					fail(res) {
						console.log('qr', res)
					}
				}); */
				myCanvas.drawImage('../../static/images/used-to-sharel-img1.png', 185*self.rpx, 180*self.rpx, 144*self.rpx, 129*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//下划线
				myCanvas.strokeStyle = "#e1e1e1";
				myCanvas.moveTo(10*self.rpx, 130*self.rpx);
				myCanvas.lineTo(340*self.rpx, 130*self.rpx);
				myCanvas.stroke();
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
			
				myCanvas.font = `normal bold ${parseInt(self.rpx * 12)}px sans-serif`;
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('长按图片，识别图中小程序码，免费查看联系方式',175.5*self.rpx,160*self.rpx,361*self.rpx);
				myCanvas.font = '11px Arial';
				myCanvas.fillStyle = '#999';
				myCanvas.fillText('没有识别出小程序信息，请重新点开图片，多试几次就行了',175.5*self.rpx,340*self.rpx,361*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//开始绘画，必须调用这一步，才会把之前的一些操作实施
				var interval = setInterval(function() {
					if (self.isCom) {
						clearInterval(interval)
				
						self.canvasToTempFilePath()
					}
				}, 1000);
				
			},
			
			canvasToTempFilePath() {
				const self = this;
				uni.canvasToTempFilePath({
					x: 0,
					y: 0,
					width: 361*self.rpx,
					height: 361*self.rpx,
					/* destWidth: 361*self.rpx,
					destHeight: 361*self.rpx, */
					canvasId: 'mycanvas',
					success: (res) => {
						console.log(res)
						this.imgurl = res.tempFilePath;
						this.savePoster()
						uni.hideLoading();
					},
				}, this);
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
						myCanvas.clearRect(0, 0, 345, 410);
						myCanvas.save()
						myCanvas.restore()
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #f5f5f5;
	}
	.wh722{width: 722rpx;height: 722rpx;}
	.pc-container image {
		width: 100%;
		min-height: 820rpx;
		border: 1px solid #BBBBBB;
		display: block;
		border-radius: 10rpx;
	}

	.pc-container {
		width: 690rpx;
		height: 820rpx;
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
		width: 260rpx;
		height: 260rpx;
	}

	.shareImg image:nth-child(2) {
		width: 288rpx;
		height: 258rpx;
	}

	.sgin {
		background-color: #D30D11;
		color: #fff;
		border-top-left-radius: 10rpx;
		border-bottom-right-radius: 10rpx;
		width: 160rpx;
		line-height: 60rpx;
		text-align: center;
		display: inline-block;
	}
</style>
