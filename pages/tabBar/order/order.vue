<template>
	<view class="main">
		<view class="imgNoOrderBox">
			<image class="imgNoOrder" src="/static/image/teamnoorder.png"></image>
		</view>
		<text class="noOrderText">没有订单信息</text>
		<view class="noOrderTip">
			<text class="noOrderTipText" @click="toHome">去首页逛逛</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		components: {},
		onLoad() {},
		methods: {
			toHome() {
				uni.switchTab({
					url: '/pages/tabBar/index/index'
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.main {
		background-color: #F6F4F5;
		height: 100vh;

		.imgNoOrderBox {
			padding-top: 100px;
			text-align: center;

			.imgNoOrder {
				width: 120px;
				height: 120px;
			}
		}

		.noOrderText {
			display: block;
			text-align: center;
			font-size: 16px;
			color: #666;
			padding: 12px 0;
		}

		.noOrderTip {
			text-align: center;
			padding-top: 24px;

			.noOrderTipText {
				color: #000000;
				font-size: 16px;
				font-weight: bold;
				padding: 12px 70px;
				background-color: #F0D71A;
				display: inline-block;
				border-radius: 30px;
			}
		}
	}
</style>
