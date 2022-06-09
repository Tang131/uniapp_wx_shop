<template>
	<view class="main">
		<navigationbar></navigationbar>
		<view class="content" :style="{'padding-top':topPadding+'px'}">
			<image class="vipImg" src="@/static/image/vip1.png"></image>
			<text class="title1">餐厅</text>
			<text class="title2" @click="toBox()">下午茶</text>
			<view class="titleBox">
				<view class="line1"></view>
				<view class="title">餐厅</view>
				<view class="line2"></view>
			</view>
			<merchantList></merchantList>
			<view class="titleBox" id="box1">
				<view class="line1"></view>
				<view class="title">下午茶</view>
				<view class="line2"></view>
			</view>
			<merchantList></merchantList>
		</view>
	</view>
</template>

<script>
	import navigationbar from '@/components/hall-food/navigationbar'
	import merchantList from '@/components/hall-food/merchantList'
	export default {
		data() {
			return {
				topPadding: 44
			}
		},
		components: {
			navigationbar,
			merchantList
		},
		onLoad() {
			let {
				statusBarHeight
			} = uni.getSystemInfoSync()
			this.topPadding = statusBarHeight + ((uni.getMenuButtonBoundingClientRect().top - statusBarHeight) * 2 + uni
				.getMenuButtonBoundingClientRect().height)
		},
		methods: {
			toBox() {
				uni.createSelectorQuery().select('#box1').boundingClientRect(
					data => { //目标位置的节点：类class或者id
						uni.pageScrollTo({
							duration: 100, //过渡时间
							scrollTop: data.top - data.height, //到达距离顶部的top值
						})
					}).exec();
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		background-color: #F6F4F5;
		height: 100vh;

		.vipImg {
			width: calc(100% - 24px);
			height: 45px;
			border-radius: 16px;
			margin: 8px 12px;
		}

		.title1 {
			font-size: 18px;
			font-weight: bold;
			margin-left: 16px;
			padding-bottom: 2px;
			// border-radius: 15px;
			border-bottom: 5px solid #f4d818;
		}

		.title2 {
			font-size: 18px;
			font-weight: bold;
			padding-left: 30px;
		}

		.titleBox {
			padding: 24px 12px;
			text-align: center;

			.line1 {
				width: 25%;
				display: inline-block;
				border-bottom: 1px solid #c4c4c4;
				vertical-align: middle;
			}

			.title {
				display: inline-block;
				padding: 0 40px;
				color: #919191;
			}

			.line2 {
				width: 25%;
				display: inline-block;
				border-bottom: 1px solid #c4c4c4;
				vertical-align: middle;
			}
		}
	}
</style>
