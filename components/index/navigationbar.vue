<template>
	<view class="nav-box">
		<view class="nav-style" :style="{'height':statusBarHeight+'px','width':windowWidth+'px'}">
		</view>
		<view class="top-tl" :style="{'height':navHeight+'px'}">
			<text class="title" :class="select_item == 1 ? 'selectTitle' : ''" @click="sel_item(1)">个人点餐</text>
			<text class="title" :class="select_item == 2 ? 'selectTitle' : ''" @click="sel_item(2)">企业团餐</text>
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
				select_item: 1
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
			sel_item(item) {
				this.select_item = item
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
			padding-left: 24px;
			align-items: center;

			.title {
				color: #6C4C00;
				background-color: #FFF193;
				font-size: 14px;
				display: inline-block;
				border-radius: 25px;
				padding: 5px 14px;
				margin-right: 8px;
			}

			.selectTitle {
				color: #FFF;
				font-weight: bold;
				background-image: linear-gradient(to right, #434343, #161616);
			}
		}
	}
</style>
