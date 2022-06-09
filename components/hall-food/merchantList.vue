<template>
	<view>
		<view class="contentBox" v-for="(item,i) in 20" :key="i" @click="toShop(item,i)">
			<image class="imgBox" src="../../static/image/img001.png"></image>
			<text class="titleBox">{{title + i}}</text>
			<image class="iconBox" @click.stop="iconClick()" :src="iconUrl"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '吉野家-科兴店',
				iconUrl: '../../static/image/collection-un.png'
			}
		},
		methods: {
			iconClick() {
				if (this.iconUrl == '../../static/image/collection-un.png') {
					this.iconUrl = '../../static/image/collection-is.png'
				} else {
					this.iconUrl = '../../static/image/collection-un.png'
				}
			},
			toShop(item, i) {
				uni.navigateTo({
					url: `/pages/shop/shop?title=${this.title + i}`
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.contentBox {
		padding: 8px;
		margin: 8px;
		border-radius: 6px;
		display: flex;
		background-color: #FFFFFF;

		.imgBox {
			width: 80px;
			height: 80px;
			display: inline-block;
			border-radius: 10px;
		}

		.titleBox {
			padding-left: 12px;
			font-size: 20px;
			font-weight: bold;
			align-self: center;
			flex: 1;
		}

		.iconBox {
			width: 22px;
			height: 22px;
			display: inline-block;
			align-items: flex-end;
			padding: 4px;
		}
	}
</style>
