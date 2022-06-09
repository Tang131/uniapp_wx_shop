<template>
	<view class="main">
		<view class="imgNoOrderBox">
			<image class="imgNoOrder" src="/static/image/teamnoorder.png"></image>
		</view>
		<text class="noOrderText">没有商品信息哦</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		components: {},
		onLoad() {},
		methods: {}
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
	}
</style>
