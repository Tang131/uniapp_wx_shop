<template>
	<view class="nav-box">
		<view class="nav-style" :style="{'height':statusBarHeight+'px','width':windowWidth+'px'}">
		</view>
		<view class="top-tl" :style="{'height':navHeight+'px'}">
			<uni-icons custom-prefix="custom-icon" type="back" size="26" class="iconBack" @click="toBack()"></uni-icons>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navHeight: '',
				statusBarHeight: '',
				windowWidth: 375,
			}
		},
		mounted() {
			let {
				windowWidth,
				statusBarHeight
			} = uni.getSystemInfoSync()
			console.log(uni.getSystemInfoSync())
			this.statusBarHeight = statusBarHeight
			this.navHeight = (uni.getMenuButtonBoundingClientRect().top - statusBarHeight) * 2 + uni
				.getMenuButtonBoundingClientRect().height
			this.windowWidth = windowWidth
		},
		methods: {
			toBack() {
				uni.navigateBack({
					delta: 1
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.nav-box {
		background-color: #F0D71A;
		position: fixed;
		z-index: 999;

		.nav-style {}

		//导航标题
		.top-tl {
			display: flex;
			align-items: center;

			.iconBack {
				padding-left: 12px;
				padding-right: 12px;
			}

			.title {
				color: #6C4C00;
				font-size: 18px;
				display: inline-block;
				padding: 5px 14px 5px 0;
				margin-right: 8px;
			}
		}
	}
</style>
