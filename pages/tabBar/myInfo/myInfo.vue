<template>
	<view class="main">
		<navigationbar></navigationbar>
		<view class="loginBox" :style="{'padding-top':topPadding+'px'}">
			<view class="loginCol">
				<image class="loginImg" src="../../../static/image/duser.png"></image>
				<text class="loginText">登录</text>
			</view>
		</view>
		<view class="listBox">
			<view class="listA">
				<uni-list>
					<uni-list-item title="团餐用券" showArrow thumb="/static/image/newminec4.png" thumb-size="sm" />
				</uni-list>
			</view>
			<uni-list class="listB">
				<uni-list-item title="联系客服" showArrow thumb="/static/image/newminec5.png" thumb-size="sm" />
				<uni-list-item title="微信客服" showArrow thumb="/static/image/newminec6.png" thumb-size="sm" rightText="[微信号: Zh_Test]" />
				<uni-list-item title="意见反馈" showArrow thumb="/static/image/newminec7.png" thumb-size="sm" />
			</uni-list>
		</view>

	</view>
</template>

<script>
	import navigationbar from '@/components/myInfo/navigationbar'
	export default {
		data() {
			return {
				topPadding: 44
			}
		},
		components: {
			navigationbar
		},
		onLoad() {
			let {
				statusBarHeight
			} = uni.getSystemInfoSync()
			this.topPadding = statusBarHeight + ((uni.getMenuButtonBoundingClientRect().top - statusBarHeight) * 2 + uni
				.getMenuButtonBoundingClientRect().height)
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	.main {
		background-color: #F6F4F5;
		height: 100vh;

		.loginBox {
			background-color: #F0D71A;
			height: 80px;

			.loginCol {
				background-color: #FFF;
				margin: 35px 12px 0 12px;
				border-radius: 6px;
				height: 110px;
				line-height: 110px;
				box-shadow: 1px 1px 5px #E7E7E7;

				.loginImg {
					width: 60px;
					height: 60px;
					vertical-align: middle;
					padding: 0 20px;
				}

				.loginText {
					font-size: 22px;
					color: #000;
					font-weight: bold;
				}
			}
		}

		.listBox {
			padding-top: 80px;
			margin: 0 12px;

			.listA {
				margin-bottom: 12px;
			}
		}
	}
</style>
