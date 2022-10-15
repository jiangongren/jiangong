<script>
import { mapMutations } from 'vuex';
// #ifdef H5
// #endif
// #ifdef APP-PLUS
import voice from '@/common/voice.js';
// #endif

// #ifdef H5
import wxAuthorize from 'common/wxAuthorize.js';
// #endif
export default {
	methods: {
		...mapMutations(['login']),
		async gzgzh(){  //关注公众号
			let openId = uni.getStorageSync('userOpenId');
			console.log(openId);
			let options = {
				url: this.$server.baseURL + "Common/GzGzh",
				loading: true,
				islogin: false,
				data: {
					openId:openId
				}
			};
			await this.$api
				.http(options)
				.then(rs => {
					console.log(rs.data);
					if (rs.data.statusCode == 400) {
						this.$api.msg(rs.data.message);
						setTimeout(() => {
							uni.navigateTo({
							    url:'/pages/index/official_account'
							});
						}, 1000)
					}
				})
				.catch(err => {
					console.log(err);
				});
		},
	},
	onLaunch: function() {
		// #ifdef H5
		let openId = uni.getStorageSync('userOpenId');
		// if(!openId){
		// 	wxAuthorize.wxAuthorize();
		// }
		// #endif
		let userInfo = uni.getStorageSync('userInfo') || '';
		if (userInfo) {
			
			//更新登陆状态
			uni.getStorage({
				key: 'userInfo',
				success: res => {
					this.login(res.data);
				}
			});
		}
		
		this.gzgzh();  //关注公众号

		// #ifdef APP-PLUS
		// 锁定屏幕方向
		plus.screen.lockOrientation('portrait-primary'); //锁定

		// 检测升级
		uni.request({
			url: this.$server.baseURL + 'update/' + plus.runtime.appid + '/update.json', //检查更新的服务器地址
			success: res => {
				//console.log('success', res);
				if (res.data.appid) {
					let osName = plus.os.name;
					let openUrl = null;
					let updateVer = plus.runtime.version;
					let version = plus.runtime.version;
					let note;
					if (osName === 'iOS') {
						openUrl = res.data.iOS.url;
						updateVer = res.data.iOS.version;
						note = res.data.iOS.note;
					} else {
						openUrl = res.data.Android.url;
						updateVer = res.data.Android.version;
						note = res.data.Android.note;
					}
					updateVer = updateVer.replace(/\./g, '');
					version = version.replace(/\./g, '');
					if (parseInt(version) < parseInt(updateVer)) {
						uni.showModal({
							title: '更新提示',
							content: note ? note : '是否选择更新',
							success: showResult => {
								if (showResult.confirm) {
									plus.runtime.openURL(openUrl);
								}
							}
						});
					}
				}
			}
		});

		//推送
		plus.push.addEventListener(
			'click',
			function(msg) {
				sxy.doClick('/pages/noticeList/personalList');
			},
			false
		);

		plus.push.addEventListener(
			'receive',
			function(msg) {
				if (msg.indexOf('蜂窝到账') != -1) {
					voice.voicePay(this.$server.baseURL, msg);
				}
			},
			false
		);

		// #endif
	},
	onShow: function() {
		console.log('App Show');

	},
	onHide: function() {
		console.log('App Hide');
	}
};
</script>

<style lang="scss">

@import 'colorui/main.css';
@import 'colorui/icon.css';
@import 'common/uni.css';
@import "./static/flex.less";
@font-face {
	font-family: yticon;
	font-weight: normal;
	font-style: normal;
	src: url('https://at.alicdn.com/t/font_1078604_w4kpxh0rafi.ttf') format('truetype');
}

.yticon {
	font-family: 'yticon' !important;
	font-size: 16px;
	font-style: normal;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

.icon-yiguoqi1:before {
	content: '\e700';
}

.icon-iconfontshanchu1:before {
	content: '\e619';
}

.icon-iconfontweixin:before {
	content: '\e611';
}

.icon-alipay:before {
	content: '\e636';
}

.icon-shang:before {
	content: '\e624';
}

.icon-shouye:before {
	content: '\e626';
}

.icon-shanchu4:before {
	content: '\e622';
}

.icon-xiaoxi:before {
	content: '\e618';
}

.icon-jiantour-copy:before {
	content: '\e600';
}

.icon-fenxiang2:before {
	content: '\e61e';
}

.icon-pingjia:before {
	content: '\e67b';
}

.icon-daifukuan:before {
	content: '\e68f';
}

.icon-pinglun-copy:before {
	content: '\e612';
}

.icon-dianhua-copy:before {
	content: '\e621';
}

.icon-shoucang:before {
	content: '\e645';
}

.icon-xuanzhong2:before {
	content: '\e62f';
}

.icon-gouwuche_:before {
	content: '\e630';
}

.icon-icon-test:before {
	content: '\e60c';
}

.icon-icon-test1:before {
	content: '\e632';
}

.icon-bianji:before {
	content: '\e646';
}

.icon-jiazailoading-A:before {
	content: '\e8fc';
}

.icon-zuoshang:before {
	content: '\e613';
}

.icon-jia2:before {
	content: '\e60a';
}

.icon-huifu:before {
	content: '\e68b';
}

.icon-sousuo:before {
	content: '\e7ce';
}

.icon-arrow-fine-up:before {
	content: '\e601';
}

.icon-hot:before {
	content: '\e60e';
}

.icon-lishijilu:before {
	content: '\e6b9';
}

.icon-zhengxinchaxun-zhifubaoceping-:before {
	content: '\e616';
}

.icon-naozhong:before {
	content: '\e64a';
}

.icon-xiatubiao--copy:before {
	content: '\e608';
}

.icon-shoucang_xuanzhongzhuangtai:before {
	content: '\e6a9';
}

.icon-jia1:before {
	content: '\e61c';
}

.icon-bangzhu1:before {
	content: '\e63d';
}

.icon-arrow-left-bottom:before {
	content: '\e602';
}

.icon-arrow-right-bottom:before {
	content: '\e603';
}

.icon-arrow-left-top:before {
	content: '\e604';
}

.icon-icon--:before {
	content: '\e744';
}

.icon-zuojiantou-up:before {
	content: '\e605';
}

.icon-xia:before {
	content: '\e62d';
}

.icon--jianhao:before {
	content: '\e60b';
}

.icon-weixinzhifu:before {
	content: '\e61a';
}

.icon-comment:before {
	content: '\e64f';
}

.icon-weixin:before {
	content: '\e61f';
}

.icon-fenlei1:before {
	content: '\e620';
}

.icon-erjiye-yucunkuan:before {
	content: '\e623';
}

.icon-Group-:before {
	content: '\e688';
}

.icon-you:before {
	content: '\e606';
}

.icon-forward:before {
	content: '\e607';
}

.icon-tuijian:before {
	content: '\e610';
}

.icon-bangzhu:before {
	content: '\e679';
}

.icon-share:before {
	content: '\e656';
}

.icon-yiguoqi:before {
	content: '\e997';
}

.icon-shezhi1:before {
	content: '\e61d';
}

.icon-fork:before {
	content: '\e61b';
}

.icon-kafei:before {
	content: '\e66a';
}

.icon-iLinkapp-:before {
	content: '\e654';
}

.icon-saomiao:before {
	content: '\e60d';
}

.icon-shezhi:before {
	content: '\e60f';
}

.icon-shouhoutuikuan:before {
	content: '\e631';
}

.icon-gouwuche:before {
	content: '\e609';
}

.icon-dizhi:before {
	content: '\e614';
}

.icon-fenlei:before {
	content: '\e706';
}

.icon-xingxing:before {
	content: '\e70b';
}

.icon-tuandui:before {
	content: '\e633';
}

.icon-zuanshi:before {
	content: '\e615';
}

.icon-zuo:before {
	content: '\e63c';
}

.icon-shoucang2:before {
	content: '\e62e';
}

.icon-shouhuodizhi:before {
	content: '\e712';
}

.icon-yishouhuo:before {
	content: '\e71a';
}

.icon-dianzan-ash:before {
	content: '\e617';
}

view,
scroll-view,
swiper,
swiper-item,
cover-view,
cover-image,
icon,
text,
rich-text,
progress,
button,
checkbox,
form,
input,
label,
radio,
slider,
switch,
textarea,
navigator,
audio,
camera,
image,
video {
	box-sizing: border-box;
}

/* 骨架屏替代方案 */
.Skeleton {
	background: #f3f3f3;
	padding: 20upx 0;
	border-radius: 8upx;
}

/* 图片载入替代方案 */
.image-wrapper {
	font-size: 0;
	background: #f3f3f3;
	border-radius: 4px;

	image {
		width: 100%;
		height: 100%;
		transition: 0.6s;
		opacity: 0;

		&.loaded {
			opacity: 1;
		}
	}
}

.clamp {
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
	display: block;
}

.common-hover {
	/* background: #f5f5f5; */
}

/*边框*/
.b-b:after,
.b-t:after {
	position: absolute;
	z-index: 3;
	left: 0;
	right: 0;
	height: 0;
	content: '';
	transform: scaleY(0.5);
	border-bottom: 1px solid $border-color-base;
}

.b-b:after {
	bottom: 0;
}

.b-t:after {
	top: 0;
}

/* button样式改写 */
uni-button,
button {
	height: 80upx;
	line-height: 80upx;
	font-size: $font-lg + 2upx;
	font-weight: normal;

	&.no-border:before,
	&.no-border:after {
		border: 0;
	}
}

uni-button[type='default'],
button[type='default'] {
	color: $font-color-dark;
}

/* input 样式 */
.input-placeholder {
	color: #999999;
}

.placeholder {
	color: #999999;
}



.no-bad-table-wrap {
		.rowClassName {
			.td {
				background: red;

				.td_wrap {
					background: red;
					color: #fff;
				}
			}
		}

		//自定义某一行样式
		.demo-table-info-row .td {
			background-color: #2db7f5;
			color: #fff;

			.td_wrap {
				background-color: #2db7f5;
				color: #fff;
			}
		}

		.demo-table-error-row .td {
			background-color: #ff6600;
			color: #fff;

			.td_wrap {
				background-color: #ff6600;
				color: #fff;
			}
		}

		.td.demo-table-info-column {
			background-color: #2db7f5;
			color: #fff;

			.td_wrap {
				background-color: #2db7f5;
				color: #fff;
			}
		}

	

		.table_box {
			.demo-table-info-cell-name {
				background-color: #2db7f5;
				color: #fff;
			
				.td_wrap {
					background-color: #2db7f5;
					color: #fff;
				}
			}
			.demo-table-info-cell-age {
				background-color: #ff6600;
				color: #fff;

				.td_wrap {
					background-color: #ff6600;
					color: #fff;
				}

			}

			.demo-table-info-cell-address {
				background-color: #187;
				color: #fff;

				.td_wrap {
					background-color: #187;
					color: #fff;
				}
			}
		}


		.btn-delete {
			background: red !important;
			color: #fff !important;
		}


		.example {
			.title {
				line-height: 40px;
				font-weight: bold;
			}
		}
	}
	
	uni-checkbox .uni-checkbox-input {
		border-radius: 50% !important;
		color: #ffffff !important;
		transform: scale(0.8);
	}
	
	uni-checkbox .uni-checkbox-input.uni-checkbox-input-checked {
		color: #fff;
		border-color: rgb(0, 122, 255);
		background: rgb(0, 122, 255);
	}
	uni-checkbox .uni-checkbox-input.uni-checkbox-input-checked:after {
		font-size: 18px;
	}
			
</style>
