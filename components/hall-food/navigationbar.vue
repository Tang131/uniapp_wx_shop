<template>
	<view class="nav-box">
		<view class="nav-style" :style="{'height':statusBarHeight+'px','width':windowWidth+'px'}">
		</view>
		<view class="top-tl" :style="{'height':navHeight+'px'}">
			<uni-icons type="location-filled" size="18"></uni-icons>
			<text class="title" @click="getLocation()">{{location? location :'点击设置配送地址'}}</text>
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
				location: ''
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
			getLocation() {
				let _this = this
				uni.chooseLocation({
					success: function(res) {
						_this.location = res.name
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
					}
				});
			},
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
				font-size: 14px;
				display: inline-block;
				padding: 5px 14px 5px 0;
				margin-right: 8px;
			}
		}
	}
</style>
