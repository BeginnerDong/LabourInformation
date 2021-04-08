<template>
	<view>

		<view class="font-24 color2 text-center bg-white py-3 head">
			<view class="Rcolor font-w pb-1 font-30">分享到朋友圈步骤</view>
			<view style="line-height: 50rpx;" class="d-flex a-center j-center">
				<view style="color:red;width: 30rpx;height:30rpx;border-radius: 50%;overflow: hidden;font-size:10px;border: 1px solid red;display: flex;align-items: center;justify-content: center;">1</view>
				<view class="pl-1 pr-1">先保存图片></view>
				<view  style="color:red;width: 30rpx;height:30rpx;border-radius: 50%;overflow: hidden;font-size:10px;border: 1px solid red;display: flex;align-items: center;justify-content: center">2</view>
				<view class="pl-1 pr-1">将图片发送到朋友圈</view>
			</view>
			<view>他人可通过点开图片长按识别小程序码，查看本条信息</view>
		</view>
		<view style="height: 20rpx;width: 100%;background-color: #f5f5f5;"></view>
		<view class="pc-container" style="position: fixed;left:0;top: 9999px;" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas" :style="{width: width+'px',height:height+'px'}"></canvas>
		</view>




		<!-- 出售求购分享 -->
		<view class="bg-white">
			<view class="py-3 font-32 color2 text-center">图片样式</view>
			<view class="rounded10 overflow-h shadow  wh722" style="margin-left: 14rpx;">
				<view class="d-flex flex-wrap">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:(mainData.behavior==2?'../../static/images/qiugou.jpg':'')" class="img1"></image>
					<view class="px-1 color2 w-50 font-34" style="height: 350rpx;overflow: hidden;line-height: 60rpx;">
						{{mainData.title?mainData.title:''}}</view>
					<view class="pt-2" style="width: 340rpx;">
						<image :src="qrUrl" class="img2"></image>
						<view class="font-26 color6  text-center font-w">长按识别图中小程序码</view>
						<view class="font-26 color6  text-center font-w">查看详情</view>
					</view>
					<view class="pt-2 px-1">
						<view class="font-32 Mcolor d-flex a-center j-sb pb-1">
							<view class="d-flex a-center">
								<image src="../../static/images/second-handl-icon1.png" class="icon4"></image>{{mainData.city&&mainData.city.title?mainData.city.title:'未知'}}
							</view>
							
							<view style="color: red;font-size: 32rpx;font-weight: 700;">{{mainData.behavior==1?'出售':'求购'}}</view>
						</view>
						<view class="font-28 color2 d-flex line-h a-center j-sb borderT-e1 py-2">
							<view class="d-flex a-center font-26">
								<image src="../../static/images/second-handl-icon2.png" class="icon5"></image>{{mainData.name?mainData.name:''}}
							</view>
							<view class="d-flex a-center font-26">
								<image src="../../static/images/second-handl-icon3.png" class="icon6"></image>{{mainData.phone?mainData.phone:''}}
							</view>
						</view>
						<view class="font-20 color6 p-1 pl-2 border-e1 txt  line-h">
							<view class="pb-15 font-w">桥隧之家-完全免费的信息平台</view>
							<view class="pb-15">您可以</view>
							<view class="pb-15">1.快速的出售/求购二手机械设备</view>
							<view class="pb-15">2.快速查询工程相关资源供应商</view>
							<view class="pb-15">3.快速招民工/班组，或者求职</view>
						</view>
					</view>
				</view>
				<view class="font-22 color9 text-center pt-1" style="letter-spacing: 1rpx;">没有识别出小程序信息，重新点开图片长按识别，多试几次就行了</view>
			</view>

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
				qrUrl: '',
				rpx: 0,
				width: 0,
				height: 0,
				isCom:false
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.qrUrl = uni.getStorageSync('secondHandQr');
			self.$Utils.loadAll(['getMainData'], self);
			wx.downloadFile({
				url: self.qrUrl, //网络路径
				success: function(res) {
					console.log('qr', res)
					self.isCom1 = true;
					self.qrCodeUrl = res.tempFilePath
				},
			});
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

			submit() {
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
						if (self.mainData && self.mainData.mainImg && self.mainData.mainImg[0]) {
							var mainImg = self.mainData.mainImg[0].url;
							wx.downloadFile({
								url: mainImg,
								success(res) {
									console.log('res', res)
									self.mainUrl = res.tempFilePath
									/* myCanvas.drawImage(res.tempFilePath, 0, 0, 175 * self.rpx, 170 * self.rpx);
									 */
									self.isCom = true;
								},
								fail(res) {
									self.$Utils.showToast('绘制主图失败' + res.errMsg, 'none');
									//self.isCom = true;
								}
							});
						}else{
							self.isCom = true;
						}
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
					ctx_2d.textAlign = 'left' //文字居中
					ctx_2d.fillStyle = '#222';
					ctx_2d.font = '17px Arial bold';
					ctx_2d.fillText(text.substr(0, tl).replace(/^\s+|\s+$/, ""), startleft, (i - 1) * lineheight + starttop);
					text = text.substr(tl);
				}
			},
			/**
			 * 
			 * @param {CanvasContext} ctx canvas上下文
			 * @param {number} x 圆角矩形选区的左上角 x坐标
			 * @param {number} y 圆角矩形选区的左上角 y坐标
			 * @param {number} w 圆角矩形选区的宽度
			 * @param {number} h 圆角矩形选区的高度
			 * @param {number} r 圆角的半径
			 */
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
				myCanvas.fillRect(0, 0, 361 * self.rpx, 361 * self.rpx);
				//myCanvas.drawImage('../../static/images/second-share.png', 0, 0, 370*self.rpx, 400*self.rpx);
				
				
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.save()
				
				if (self.mainUrl) {
					myCanvas.drawImage(self.mainUrl, 0, 0, 175 * self.rpx, 165 * self.rpx);
				}else{
					myCanvas.drawImage('../../static/images/qiugou.jpg', 0, 0, 175 * self.rpx, 165 * self.rpx);
				};
				//return
				//myCanvas.drawImage(mainImg, 0, 0, 165, 165);
				//参数：图片，左偏移，上偏移，宽，高
				//二维码
				myCanvas.drawImage(self.qrCodeUrl, 30 * self.rpx, 180 * self.rpx, 115 * self.rpx, 115 * self.rpx);
				
				//myCanvas.drawImage('../../static/images/used to sharel-img.png', 30, 180, 115, 115);
				myCanvas.fillStyle = '#666';
				//myCanvas.font = `11px Arial bold`;
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = `normal bold ${parseInt(self.rpx * 11)}px sans-serif`;
				myCanvas.fillText('长按识别图中小程序码', 90 * self.rpx, 315 * self.rpx);
				myCanvas.fillText('查看详情', 90 * self.rpx, 335 * self.rpx);
				//描述
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				
				
				//描述
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var content = this.mainData.title;
				if(content.length>55){
					this.writeTextOnCanvas(myCanvas, 25 * self.rpx, 20 * self.rpx, content.substr(0,53)+'...', 180 * self.rpx, 20 * self.rpx)
				}else{
					this.writeTextOnCanvas(myCanvas, 25 * self.rpx, 20 * self.rpx, content, 180 * self.rpx, 20 * self.rpx)
				}
				
				/* writeTextOnCanvas(ctx_2d, lineheight, bytelength, text, startleft, starttop) */
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//所在地

				var location = this.mainData.city.title ? this.mainData.city.title : '未知';
				myCanvas.drawImage('../../static/images/second-handl-icon1.png', 175 * self.rpx, 175 * self.rpx, 11 * self.rpx, 13 *
					self.rpx);
				myCanvas.fillStyle = '#51A9E9';
				myCanvas.font = '16px Arial';
				myCanvas.fillText(location, 192 * self.rpx, 186 * self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//下划线
				
				myCanvas.fillStyle = '#D31114';
				myCanvas.font = `17px Arial bold`;
				myCanvas.fillText(this.mainData.behavior==1?'出售':'求购', 320 * self.rpx, 186 * self.rpx);
				myCanvas.strokeStyle = "#e1e1e1";
				myCanvas.moveTo(175 * self.rpx, 195.5 * self.rpx);
				myCanvas.lineTo(355 * self.rpx, 195.5 * self.rpx);
				myCanvas.stroke();
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//联系人信息
				var name = this.mainData.name;
				myCanvas.drawImage('../../static/images/second-handl-icon2.png', 175 * self.rpx, 205 * self.rpx, 11 * self.rpx, 14 *
					self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.fillStyle = '#000';
				myCanvas.font = '14px Arial';
				myCanvas.fillText(name, 192 * self.rpx, 217 * self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var phone = this.mainData.phone;
				myCanvas.drawImage('../../static/images/second-handl-icon3.png', 240 * self.rpx, 205 * self.rpx, 11 * self.rpx, 14 *
					self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.fillStyle = '#000';
				myCanvas.font = '14px Arial';
				myCanvas.fillText(phone, 253 * self.rpx, 217 * self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//平台介绍	
				myCanvas.strokeRect(175.5 * self.rpx, 230.5 * self.rpx, 180 * self.rpx, 105 * self.rpx);
				myCanvas.textAlign = 'left' //文字居中
				myCanvas.fillStyle = '#666';
				
				myCanvas.font = `normal bold ${parseInt(self.rpx * 11)}px sans-serif`;
				myCanvas.fillText('桥隧之家-完全免费的信息平台', 180 * self.rpx, 250 * self.rpx);
				myCanvas.font = '11px Arial';
				myCanvas.fillText('您可以', 180 * self.rpx, 270 * self.rpx);
				myCanvas.fillText('1.快速的出售/求购二手机械设备', 180 * self.rpx, 290 * self.rpx);
				myCanvas.fillText('2.快速查询工程相关资源供应商', 180 * self.rpx, 310 * self.rpx);
				myCanvas.fillText('3.快速招民工/班组，或者求职', 180 * self.rpx, 330 * self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.fillStyle = '#999';
				myCanvas.fillText('没有识别出小程序信息，请重新点开图片，多试几次就行了', 180 * self.rpx, 355 * self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				//self.roundRect(myCanvas,0,0,345 * self.rpx, 400 * self.rpx,10);
				//开始绘画，必须调用这一步，才会把之前的一些操作实施
				var interval = setInterval(function() {
					if (self.isCom && self.isCom1) {
						clearInterval(interval)
							
						setTimeout(function() {
							self.canvasToTempFilePath()
						}, 300);		
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
						console.log('222',res)
						this.imgurl = res.tempFilePath;
						//this.canvasShow = true
						this.savePoster()
						uni.hideLoading();
					},
					fail(err) {
						console.log('333',err)
					}
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
	.pb-15{padding-bottom: 15rpx;}
	.wh722{width: 722rpx;height: 722rpx;}
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
		width: 19rpx;
		height: 24rpx;
		margin-right: 10rpx;
	}

	.icon6 {
		width: 15rpx;
		height: 23rpx;
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
		width: 360rpx;
		height: 180rpx;
	}

	.img1 {
		width: 350rpx;
		height: 350rpx;
	}

	.img2 {
		width: 232rpx;
		height: 214rpx;
		margin: 0 auto;
	}
</style>
