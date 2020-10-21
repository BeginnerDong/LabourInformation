<template>
	<view>

		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-3">分享到朋友圈步骤</view>
			<view><text>1</text>先保存图片><text>2</text>将图片发送到朋友圈</view>
			<view>他人可通过点开图片长按识别小程序码，查看本条信息</view>
		</view>
		<view style="height: 20rpx;width: 100%;background-color: #f5f5f5;"></view>
		
		

		<!-- 公司分享 -->
		<view class="bg-white  px-3">
			<view class="py-3 font-28 color2 d-flex a-center j-sa">
				<view class="d-flex a-center" @click="change(1)">
					<image :src="current==1?'../../static/images/used-to-sharel-icon1.png':'../../static/images/used-to-sharel-icon.png'"
					 class="icon1"></image>图片样式1
				</view>
				<view class="d-flex a-center" @click="change(2)">
					<image :src="current==2?'../../static/images/used-to-sharel-icon1.png':'../../static/images/used-to-sharel-icon.png'"
					 class="icon1"></image>图片样式2
				</view>
			</view>
			<view class="rounded10 overflow-h shadow pb-3">
				<view class="colorf bg-colorR py-4">
					<view class="font-50 text-center pb-3">更多二手供需信息</view>
					<view class="font-34 text-center" v-if="current==1">进入我的主页查看</view>
					<view class="d-flex a-center j-center" v-if="current==2">
						<view class="font-28 d-flex a-center">
							<image src="../../static/images/used-to-sharel-icon2.png" class="icon2"></image>{{info.name?info.name:''}}
						</view>
						<view class="font-28 d-flex a-center ml-5">
							<image src="../../static/images/used-to-sharel-icon3.png" class="icon3"></image>{{info.phone?info.phone:''}}
						</view>
					</view>
				</view>
				<view class="shareImg d-flex j-sb a-center px-3 pt-4">
					<image :src="qrUrl" mode=""></image>
					<image src="../../static/images/used-to-sharel-img1.png" mode=""></image>
				</view>
				<view class="Rcolor font-36 text-center py-3">长按图片，识别图中小程序</view>
				<view class="font-24 color6 text-right d-flex a-center j-sa">
					<view class="borderR-e1 px-1">如果长安图片没有识别出小程序信息，<br />请重新点开图片，在长安识别，多试几次就行了</view>
					<image src="../../static/images/used-to-sharel-img2.png" class="img"></image>
				</view>
			</view>

			<view class="py-5">
				<view class="btn400" @click="submit">保存图片</view>
			</view>
		</view>
		
		<view class="pc-container" style="position: fixed;left:0;top: 9999px" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas" :style="{width: width+'px',height:height+'px'}" v-show="canvasShow"></canvas>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgurl: '',
				canvasShow: true,
				userData: {},
				current: 1,
				qrUrl:'',
				info:{},
				rpx: 0,
				width: 0,
				height: 0,
			}
		},

		onLoad(options) {
			const self = this;
			self.user_no = options.user_no;
			self.qrUrl = uni.getStorageSync('userQr');
			self.$Utils.loadAll(['getUserData'], self);
			wx.getSystemInfo({
				success: function(res) {
					self.rpx = res.windowWidth / 375;
					self.width = 345 * self.rpx
					self.height = 400 * self.rpx
					console.log('self.width', self.width)
				},
			})
		},

		computed: {

		},

		watch: {

		},


		methods: {

			change(type) {
				const self = this;
				if (self.current != type) {
					self.current = type;
					
				}
			},

			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: self.user_no,
					user_type: 0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						self.info = self.userData.info;
						//self.getRelationData()
						//this.canvasImage()
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
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
				var myCanvas = uni.createCanvasContext('mycanvas', this);
				//画布尺寸
				//myCanvas.draw()//清空原来的画图内容
				//myCanvas.save();
				// 坐标(0,0) 表示从此处开始绘制，相当于偏移。
				//背景色
				myCanvas.fillStyle = "#FFFFFF";
				myCanvas.fillRect(0, 0, 345*self.rpx, 400*self.rpx);
				myCanvas.fillStyle = "#D31114";
				myCanvas.fillRect(0, 0, 345*self.rpx, 110*self.rpx);
				//类型
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = '25px Arial'
				myCanvas.fillStyle = '#fff';
				myCanvas.fillText('更多二手供需信息', 175.5*self.rpx, 50*self.rpx, 345*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				if (this.current == 1) {
					myCanvas.font = '17px Arial'
					myCanvas.fillText('进入我的主页查看', 175.5*self.rpx, 90*self.rpx, 345*self.rpx);
				} else {
					myCanvas.drawImage('../../static/images/used-to-sharel-icon2.png', 80*self.rpx, 72*self.rpx, 15*self.rpx, 16*self.rpx);
					myCanvas.font = '15px Arial'
					myCanvas.fillText(this.info.name, 125*self.rpx, 85*self.rpx);

					myCanvas.drawImage('../../static/images/used-to-sharel-icon3.png', 170*self.rpx, 72*self.rpx, 11*self.rpx, 15*self.rpx);
					myCanvas.fillText(this.info.phone, 230*self.rpx, 85*self.rpx);
				}
				//内容
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				wx.getImageInfo({
					src:this.qrUrl,
					complete: (res) => {
						self.isCom = true;
						myCanvas.drawImage(res.path, 30*self.rpx,
							140*self.rpx, 130*self.rpx, 130*self.rpx);
							//myCanvas.draw();
							myCanvas.save()
							myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
							myCanvas.draw(true)
					}
				});
				myCanvas.drawImage('../../static/images/used-to-sharel-img1.png', 170*self.rpx, 140*self.rpx, 161*self.rpx, 145*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = '18px Arial'
				myCanvas.fillStyle = '#D31114';
				myCanvas.fillText('长按图片，识别图中小程序', 175.5*self.rpx, 320*self.rpx, 345*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'start' //文字居中
				myCanvas.font = '12px Arial'
				myCanvas.fillStyle = '#666';
				myCanvas.fillText('如果长按图片没有识别出小程序信息，', 60*self.rpx, 355*self.rpx);
				myCanvas.fillText('请重新点开图片，在长按识别，多试几次就行了', 5*self.rpx, 370*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.strokeStyle = "#e1e1e1";
				myCanvas.moveTo(265.5*self.rpx, 345*self.rpx);
				myCanvas.lineTo(265.5*self.rpx, 375*self.rpx);
				myCanvas.stroke();
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.drawImage('../../static/images/used-to-sharel-img2.png', 270*self.rpx, 345*self.rpx, 75*self.rpx, 31*self.rpx);
				//开始绘画，必须调用这一步，才会把之前的一些操作实施
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//myCanvas.restore()
				self.roundRect(myCanvas,0,0,345 * self.rpx, 400 * self.rpx,10);
				//开始绘画，必须调用这一步，才会把之前的一些操作实施
				var interval = setInterval(function() {
					if (self.isCom) {
						clearInterval(interval)
				
						self.canvasToTempFilePath()
					}
				}, 1000);

			},
			
			roundRect(ctx, x, y, w, h, r) {
				// 开始绘制
				ctx.beginPath()
				// 因为边缘描边存在锯齿，最好指定使用 transparent 填充
				// 这里是使用 fill 还是 stroke都可以，二选一即可
				ctx.setFillStyle('transparent')
				// ctx.setStrokeStyle('transparent')
				// 左上角
				ctx.arc(x + r, y + r, r, Math.PI, Math.PI * 1.5)
			
				// border-top
				ctx.moveTo(x + r, y)
				ctx.lineTo(x + w - r, y)
				ctx.lineTo(x + w, y + r)
				// 右上角
				ctx.arc(x + w - r, y + r, r, Math.PI * 1.5, Math.PI * 2)
			
				// border-right
				ctx.lineTo(x + w, y + h - r)
				ctx.lineTo(x + w - r, y + h)
				// 右下角
				ctx.arc(x + w - r, y + h - r, r, 0, Math.PI * 0.5)
			
				// border-bottom
				ctx.lineTo(x + r, y + h)
				ctx.lineTo(x, y + h - r)
				// 左下角
				ctx.arc(x + r, y + h - r, r, Math.PI * 0.5, Math.PI)
			
				// border-left
				ctx.lineTo(x, y + r)
				ctx.lineTo(x + r, y)
			
				// 这里是使用 fill 还是 stroke都可以，二选一即可，但是需要与上面对应
				ctx.fill()
				// ctx.stroke()
				ctx.closePath()
				// 剪切
				console.log(111)
				ctx.clip()
				ctx.save()
				ctx.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				ctx.draw(true)
			},
			
			submit(){
				this.canvasImage()
			},
			
			canvasToTempFilePath() {
				//const self = this;
				uni.canvasToTempFilePath({
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
				console.log()
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
						myCanvas.clearRect(0, 0, 345, 380);
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
		min-height: 760rpx;
		border: 1px solid #BBBBBB;
		display: block;
		border-radius: 10rpx;
	}

	.pc-container {
		width: 690rpx;
		height: 760rpx;
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
		width: 28rpx!important;
		height: 28rpx!important;
		margin-right: 20rpx;
	}

	.icon2 {
		width: 30rpx!important;
		height: 32rpx!important;
		margin-right: 10rpx;
	}

	.icon3 {
		width: 22rpx!important;
		height: 30rpx!important;
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
