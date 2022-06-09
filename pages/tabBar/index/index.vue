<template>
	<view class="main">
		<navigationbar></navigationbar>
		<view class="searchBox" :style="{'padding-top':topPadding+'px'}">
			<uni-search-bar placeholder="请搜索商家选择下单" @confirm="search" @cancel="cancel" cancel-text="取消"
				:radius="100">
				<uni-icons slot="searchIcon" color="#999999" size="18" type="search" />
			</uni-search-bar>
			<view class="searchTip">
				<uni-icons class="icon" type="sound-filled" size="22"></uni-icons>
				<text class="text">个人晚餐暂不开放</text>
			</view>
		</view>
		<view class="topTitle">
			<text class="title">今日Top10</text>
			<text class="not">科兴区实时销量榜</text>
		</view>
		<view class="topView">
			<view class="topBoxA">
				<image class="topImg" src="@/static/image/top01.png"></image>
			</view>
			<view class="topBox" v-for="(item,i) in 10" :key="i">
				<image class="imgBox" src="@/static/image/top1.png"></image>
				<text class="titleBox">{{title + i}}</text>
				<text class="explain">{{explain + i}}</text>
				<view class="moneyBox">
					<text class="money"><text class="money_icon">￥</text>{{money}}</text>
					<text class="money_button">下单</text>
				</view>
			</view>
		</view>
		<view class="contentBox">
			<text class="titleA">餐厅推荐</text>
			<merchantList></merchantList>
		</view>
		<dragball :x='fixcartX' :y='fixcartY' image="/static/image/fixcart.png" @toFixcart="toFixcart()"></dragball>
	</view>
</template>

<script>
	import merchantList from '@/components/index/merchantList'
	import navigationbar from '@/components/index/navigationbar'
	import dragball from "@/components/drag-ball/drag-ball.vue";
	export default {
		data() {
			return {
				title: '吉野家-科兴店',
				explain: '老味道-吉野家',
				money: '20.00',
				topPadding: 44,
				fixcartX: 0,
				fixcartY: 0
			}
		},
		components: {
			merchantList,
			navigationbar,
			dragball
		},
		onLoad() {
			console.log('index')
			let {
				statusBarHeight
			} = uni.getSystemInfoSync()
			this.topPadding = statusBarHeight + ((uni.getMenuButtonBoundingClientRect().top - statusBarHeight) * 2 + uni
				.getMenuButtonBoundingClientRect().height)
			this.fixcartX = uni.getSystemInfoSync().windowWidth - 100
			this.fixcartY = uni.getSystemInfoSync().windowHeight - 100
		},
		methods: {
			getLocation() {
				uni.chooseLocation({
					success: function(res) {
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
					}
				});
			},
			search(res) {
				uni.showToast({
					title: '搜索：' + res.value,
					icon: 'none'
				})
			},
			input(res) {
				console.log('----input:', res)
			},
			clear(res) {
				uni.showToast({
					title: 'clear事件，清除值为：' + res.value,
					icon: 'none'
				})
			},
			blur(res) {
				uni.showToast({
					title: 'blur事件，输入值为：' + res.value,
					icon: 'none'
				})
			},
			focus(e) {
				uni.showToast({
					title: 'focus事件，输出值为：' + e.value,
					icon: 'none'
				})
			},
			cancel(res) {
				uni.showToast({
					title: '点击取消，输入值为：' + res.value,
					icon: 'none'
				})
			},
			toFixcart() {
				uni.navigateTo({
					url: '/pages/shoppingCart/shoppingCart?name=uniapp'
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.main {
		background-color: #F6F4F5;

		.searchBox {
			background-color: #F0D71A;
			border-radius: 0px 0px 30px 30px;
			padding-bottom: 12px;

			.search-result {
				padding-top: 10px;
				padding-bottom: 20px;
				text-align: center;
			}

			.search-result-text {
				text-align: center;
				font-size: 14px;
				color: #666;
			}

			.example-body {
				/* #ifndef APP-NVUE */
				display: block;
				/* #endif */
				padding: 0px;
			}

			.uni-mt-10 {
				margin-top: 10px;
			}

			.searchTip {
				height: 25px;
				line-height: 25px;

				.text {
					font-size: 14px;
				}

				.icon {
					padding: 0 6px 0 12px;
					vertical-align: middle;
				}
			}
		}

		.topTitle {
			padding: 12px 12px 0;

			.title {
				font-size: 16px;
				color: #000000;
				font-weight: bold;
				padding-right: 12px;
			}

			.not {
				font-size: 12px;
				color: #666666;
			}
		}

		.topView {
			padding: 8px 12px 0;
			height: 240px;
			overflow-x: scroll;
			width: auto;
			white-space: nowrap;

			.topBoxA {
				display: inline-block;
				margin: 8px 0 0;

				.topImg {
					width: 80px;
					height: 222px;
					vertical-align: bottom;
					display: inline-block;
					border-radius: 10px;
				}
			}

			.topBox {
				padding: 8px;
				margin: 8px 8px 0 8px;
				border-radius: 6px;
				background-color: #FFFFFF;
				display: inline-block;

				.imgBox {
					width: 150px;
					height: 150px;
					display: block;
					border-radius: 10px;
				}

				.titleBox {
					font-size: 14px;
					font-weight: bold;
					display: block;
					padding-bottom: 4px;
				}

				.explain {
					font-size: 12px;
					display: block;
					color: #999999;
					padding-bottom: 4px;
				}

				.moneyBox {
					display: flex;

					.money {
						flex: 1;
						color: #e9604a;
						font-weight: bold;

						.money_icon {
							font-size: 9px;
						}
					}

					.money_button {
						color: #000000;
						background-color: #f4d818;
						border-radius: 6px;
						padding: 1px 2px;
						font-size: 12px;
						display: inline-block;
						text-align: center;
						width: 35px;
						height: 20px;
						line-height: 20px;
					}
				}
			}
		}



		.contentBox {
			.titleA {
				display: block;
				padding: 16px 12px;
				font-size: 20px;
				font-weight: 800;
			}
		}

	}
</style>
