<template>
	<view class="main">
		<navigationbar></navigationbar>
		<view class="shopInfoBox" :style="{'padding-top':topPadding+'px','width':windowWidth+'px'}">
			<view class="shopInfo">
				<image class="shopImg" src="/static/image/img001.png"></image>
				<text class="shopTitle">{{title}}</text>
			</view>

			<view class="store-container">
				<view class="food">
					<view class="food-class" :style="'height:' + listHeight + 'px'">
						<scroll-view scroll-y scroll-with-animation :scroll-into-view="viewToLeft"
							:style="'height:' + listHeight + 'px'">
							<view :class="'food-class-item ' + (activeIndex == index ? 'active' : '')" @tap="selectFood"
								:data-index="index" :data-titleId="item.titleId" :id="item.titleId + 'left'"
								v-for="(item, index) in food" :key="item.title">
								<view class="text" :data-index="index" :data-titleId="item.titleId">
									{{ item.goods_type_name }}
								</view>

								<view class="dot" v-if="item.foodCount > 0">{{ item.foodCount }}</view>
							</view>
						</scroll-view>
					</view>
					<view class="food-detail">
						<scroll-view scroll-y scroll-with-animation
							:style="'height:' + listHeight + 'px;padding-bottom:110rpx;'" :scroll-into-view="viewTo"
							@scroll="scroll">
							<view class="title-group" v-for="(item, groupindex) in food" :key="item.name">
								<view class="title" :id="item.titleId">{{ item.goods_type_name }}</view>

								<view class="food-info" v-for="(item, index) in item.items" :key="item.goodId">
									<view class="img">
										<image :src="item.goods_img" @tap="preview" :data-url="item.goods_img"></image>
									</view>

									<view class="info">
										<view class="name">{{ item.goods_name }}</view>
										<view class="price-count">
											<view class="price">￥{{ item.price }}</view>
											<view class="count">
												<view class="add btns" @tap="add" :data-item="item"
													:data-groupindex="groupindex" :data-index="index">+</view>
												<view class="num">{{ item.count }}</view>
												<view class="ruduce btns" @tap="reduce" :data-item="item"
													v-if="item.count > 0 ? true : false" :data-groupindex="groupindex"
													:data-index="index">
													-
												</view>
											</view>
										</view>
									</view>
								</view>
							</view>
							<view class="notData" v-if="food.length==0">
								<image class="icon" src="/static/image/teamnoorder.png" mode="widthFix" />
								暂无商品
							</view>
						</scroll-view>
					</view>
				</view>

				<view class="cart">
					<view class="cart-container">
						<view :class="'cart-img ' + (cart.length > 0 ? 'has-food' : '')" @tap="listCart">
							<image
								:src="cart.length > 0 ? '/static/image/gouwuche-black.png' : '/static/image/gouwuche.png'">
							</image>
						</view>
						<view class="del-price-money">
							<view class="money">￥{{ totalMoney }}</view>
						</view>
						<view v-if="status == 2 || time_status == 0" class="start-del end">已打烊</view>
						<!-- parseEventDynamicCode -->
						<view v-else @click="check"
							:class="'start-del ' + (totalMoney >= starting_fee && cart.length > 0 ? 'has-food' : '')">
							{{ totalMoney >= starting_fee && cart.length > 0 ? '去结算' : '起送费'+starting_fee }}
						</view>
					</view>
					<view class="cart-list" v-if="showCart">
						<view class="cart-list-header" @tap="listCart">关闭</view>
						<view class="cart-list-item">
							<view class="item" v-for="(item, index) in cart" :key="index">
								<view class="cart-detail">{{ item.goods_name }}</view>

								<view class="cart-detail" style="color: red">￥{{ item.price }}</view>

								<view class="cart-detail cart-count">
									<view class="add btns" @tap="reduce" :data-id="item.id"
										:data-groupindex="item.groupindex" :data-index="item.index">-</view>
									{{ item.count }}
									<view class="add btns" @tap="add" :data-id="item.id"
										:data-groupindex="item.groupindex" :data-index="item.index">+</view>
								</view>
							</view>

						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import navigationbar from '@/components/shop/navigationbar'
	let timer;
	export default {
		data() {
			return {
				title: '',
				windowWidth: 375,
				topPadding: 44,
				listHeight: 650,
				viewTo: '',
				viewToLeft: '',
				heigthArr: [],
				activeIndex: 0,
				showCart: false,
				totalMoney: 0,
				status: 1,
				time_status: 1,
				groupindex: 0,
				starting_fee: 20,
				cart: [],
				result: [],
				food: [{
						goods_type_name: '美食1',
						foodCount: 0,
						titleId: 'title1',
						foodId: 1,
						items: [{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.99,
								count: 0,
								foodId: 1,
								id: 1
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 2
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 3
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 4
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 5
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 6
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 7
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 8
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 1,
								id: 9
							}
						]
					},
					{
						goods_type_name: '美食2',
						foodCount: 0,
						titleId: 'title2',
						foodId: 2,
						items: [{
								goods_img: '/static/image/top1.png',
								goods_name: '草莓',
								price: 30.00,
								count: 0,
								foodId: 2,
								id: 1
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 2,
								id: 2
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 2,
								id: 3
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 2,
								id: 4
							},
							{
								goods_img: '/static/image/top1.png',
								goods_name: '西瓜',
								price: 10.00,
								count: 0,
								foodId: 2,
								id: 5
							}
						]
					},
					{
						goods_type_name: '美食3',
						foodCount: 0,
						titleId: 'title3',
						foodId: 3,
						items: [{
							goods_img: '/static/image/top1.png',
							goods_name: '草莓',
							price: 30.00,
							count: 0,
							foodId: 3,
							id: 1
						}]
					},
					{
						goods_type_name: '美食4',
						foodCount: 0,
						titleId: 'title4',
						foodId: 4,
						items: [{
							goods_img: '/static/image/top1.png',
							goods_name: '草莓',
							price: 30.00,
							count: 0,
							foodId: 4,
							id: 1
						}]
					},
					{
						goods_type_name: '美食5',
						foodCount: 0,
						titleId: 'title5',
						foodId: 5,
						items: [{
							goods_img: '/static/image/top1.png',
							goods_name: '草莓',
							price: 30.00,
							count: 0,
							foodId: 5,
							id: 1
						}]
					},
					{
						goods_type_name: '美食6',
						foodCount: 0,
						titleId: 'title6',
						foodId: 6,
						items: [{
							goods_img: '/static/image/top1.png',
							goods_name: '草莓',
							price: 30.00,
							count: 0,
							foodId: 6,
							id: 1
						}]
					}
				],
			}
		},
		components: {
			navigationbar
		},
		onLoad(obj) {
			console.log(obj)
			this.title = obj.title
			this.calculateHeight()
		},
		/**
		 * 生命周期函数--监听页面初次渲染完成
		 */
		onReady: function() {
			let {
				windowWidth,
				statusBarHeight,
				windowHeight
			} = uni.getSystemInfoSync()
			this.topPadding = statusBarHeight + ((uni.getMenuButtonBoundingClientRect().top - statusBarHeight) * 2 +
				uni.getMenuButtonBoundingClientRect().height)
			this.windowWidth = windowWidth
			//减去头部高度 statusBarHeight - this.topPadding - 10 减去店铺名高度 80 减去paddingTop 11高度
			this.listHeight = windowHeight - statusBarHeight - this.topPadding - 10 - 80 - 6
		},
		methods: {
			selectFood(e) {
				console.log('ji', e.target);
				this.activeIndex = e.target.dataset.index
				this.viewTo = e.target.dataset.titleid
			},
			// 手机端有延迟 节流函数效果不好 用防抖函数凑合
			scroll(e) {
				clearTimeout(timer);
				timer = setTimeout(() => {
					let srollTop = e.detail.scrollTop;
					// console.log(srollTop)
					// console.log(this.heigthArr)
					for (let i = 0; i < this.heigthArr.length; i++) {
						if (srollTop >= this.heigthArr[i] && srollTop < this.heigthArr[i + 1] && this
							.activeIndex != i) {
							this.activeIndex = i

							if (i < 3) {
								this.viewToLeft = 'title1left'
							} else {
								this.viewToLeft = 'title' + (i - 2) + 'left'
							}

							return;
						}
					}
				}, 100);
			},
			add(e) {
				// debugger
				let groupindex = e.target.dataset.groupindex;
				let index = e.target.dataset.index;
				let countMsg = 'food[' + groupindex + '].items[' + index + '].count';
				let count = this.food[groupindex].items[index].count;
				let foodCountMsg = 'food[' + groupindex + '].foodCount';
				let foodCount = this.food[groupindex].foodCount;
				let foodId = this.food[groupindex].items[index].foodId;
				count += 1;
				foodCount += 1;
				this.food[groupindex].foodCount = foodCount;
				this.food[groupindex].items[index].count = count;
				let cart = this.cart;
				// console.log('购物车', cart);
				let hasCart = false;
				var object = {};
				var result = this.result;

				for (var i = 0; i < cart.length; i++) {
					if (cart[i].foodId == foodId) {
						hasCart = true;
						break;
					}
				}

				if (hasCart) {
					cart[i].count++;
					result[i].nums++;
				} else {
					cart.push({
						...this.food[groupindex].items[index],
						groupindex,
						index
					});
					object.ids = this.food[groupindex].items[index].id;
					object.nums = this.food[groupindex].items[index].count;
					result.push(object);
				}

				let totalMoney = this.totalMoney;
				totalMoney = parseFloat(totalMoney) + parseFloat(this.food[groupindex].items[index].price);
				totalMoney = totalMoney.toFixed(2);
				// console.log('商品', cart); // console.log("总额", totalMoney)
				// console.log("价格", this.data.food[groupindex].items[index].price);
				// console.log("单个商品", this.data.food[groupindex].items[index])
				// if (list[i].num > 0) {
				// }

				// console.log(result);
				this.cart = cart
				this.totalMoney = totalMoney

			},
			reduce(e) {
				let groupindex = e.target.dataset.groupindex;
				let index = e.target.dataset.index;
				var result = this.result;
				let countMsg = 'food[' + groupindex + '].items[' + index + '].count';
				let count = this.food[groupindex].items[index].count;
				let foodCountMsg = 'food[' + groupindex + '].foodCount';
				let foodCount = this.food[groupindex].foodCount;
				let foodId = this.food[groupindex].items[index].foodId;
				count -= 1;
				foodCount -= 1;
				this.food[groupindex].foodCount = foodCount;
				this.food[groupindex].items[index].count = count;
				let cart = this.cart;

				for (var i = 0; i < cart.length; i++) {
					if (cart[i].foodId == foodId) {
						if (cart[i].count == 1) {
							cart.splice(i, 1);
							result.splice(i, 1);
						} else {
							cart[i].count--;
							result[i].nums--;
						}

						break;
					}
				}

				if (cart.length == 0) {
					this.showCart = false
				}

				let totalMoney = this.totalMoney;
				totalMoney = (totalMoney - this.food[groupindex].items[index].price).toFixed(2);
				this.cart = cart
				this.totalMoney = totalMoney
			},
			listCart() {
				if (this.cart.length > 0) {
					this.showCart = !this.showCart
				}
			},
			check() {
				if ((Number(this.totalMoney) < Number(this.starting_fee)) || this.cart.length < 0) {
					uni.showToast({
						title: '未满起送费',
						icon: 'none'
					})
					return false
				}
				// var that = this;
				// uni.setStorageSync('json', that.result);
				// uni.navigateTo({
				// 	url: '/gc_school/pages/public/index?type=3&id='
				// })
				// console.log('结算商品', that.result);
			},
			preview(e) {
				var tmp = [];
				tmp.push(e.target.dataset.url);
				var current = e.target.dataset.url;
				uni.previewImage({
					current: current,
					// 当前显示图片的http链接
					urls: tmp // 需要预览的图片http链接列表
				});
			},
			calculateHeight() {
				let heigthArr = [];
				let height = 0;
				heigthArr.push(height);
				var query = uni.createSelectorQuery();
				query.selectAll('.title-group').boundingClientRect();
				query.exec((res) => {
					for (let i = 0; i < res[0].length; i++) {
						// console.log(res[0][i]);
						height += parseInt(res[0][i].height);
						heigthArr.push(height);
					}
					this.heigthArr = heigthArr
				});
			},
		}
	}
</script>

<style lang="scss" scoped>
	.main {
		background-color: #F6F4F5;
		height: 100vh;

		.shopInfoBox {
			background-color: #F0D71A;
			height: 80px;
			position: fixed;

			.shopInfo {
				background-color: #FFF;
				// margin: 0px 8px 0 8px;
				// border-radius: 6px;
				height: 80px;
				line-height: 80px;
				box-shadow: 1px 1px 5px #E7E7E7;

				.shopImg {
					width: 40px;
					height: 40px;
					vertical-align: middle;
					padding: 0 20px;
				}

				.shopTitle {
					font-size: 22px;
					color: #000;
					font-weight: bold;
				}
			}
		}

		.food {
			display: flex;
			height: 100%;
			border-top: 1px solid #eee;
		}

		.food-class {
			background-color: #f5f9fc;
			/* background-color: #eee; */
			width: 180rpx;
		}

		.food-class .active {
			background-color: #fff;
		}

		.food-class .food-class-item {
			position: relative;
			height: 100rpx;
			box-sizing: border-box;
			padding: 10rpx 20rpx;
			font-size: 28rpx;
			line-height: 40rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 180rpx;
		}

		.food-class .food-class-item:last-child {
			margin-bottom: 120rpx;
		}

		.food-class .food-class-item .text {
			text-align: center;
			max-height: 80rpx;
			overflow: hidden;
		}

		.food-class .food-class-item .dot {
			position: absolute;
			width: 30rpx;
			height: 30rpx;
			text-align: center;
			line-height: 30rpx;
			background-color: red;
			right: 4rpx;
			top: 4rpx;
			border-radius: 50%;
			color: #fff;
			font-size: 20rpx;
		}

		.food-detail {
			flex-grow: 1;
			padding: 0 14rpx;
			font-size: 26rpx;
			background-color: #FFF;
		}

		.food-detail .title {
			font-weight: 700;
			padding: 0 20rpx;
			margin-bottom: 20rpx;
			height: 50rpx;
			line-height: 50rpx;
			/* background-color: #eee; */
			overflow: hidden;
			white-space: nowrap;
			text-overflow: ellipsis;
			margin-top: 30rpx;
		}

		.food-detail .title-group {
			padding-bottom: 30rpx;
		}

		.food-detail .title-group:last-child {
			margin-bottom: 120rpx;
		}

		::-webkit-scrollbar {
			width: 0;
			height: 0;
			color: transparent;
		}

		.food-info {
			margin-bottom: 30rpx;
			display: flex;
		}

		.food-info:last-child {
			margin-bottom: 0;
		}

		.food-info .img {
			width: 120rpx;
			text-align: center;
		}

		.food-info .img image {
			width: 100rpx;
			height: 100rpx;
			border-radius: 15rpx;
		}

		.food-info .info {
			flex-grow: 1;
			margin-left: 10rpx;
		}

		.info .name {
			font-size: 28rpx;
			/* font-weight: 700; */
		}

		.info .note {
			overflow: hidden;
			white-space: nowrap;
			text-overflow: ellipsis;
		}

		.price-count {
			display: flex;
			height: 50rpx;
			line-height: 50rpx;
		}

		.price-count .price {
			font-size: 30rpx;
			flex-grow: 1;
			color: red;
		}

		.price-count .count {
			display: flex;
			line-height: 60rpx;
			flex-grow: 1;
			flex-direction: row-reverse;
		}

		.price-count .count .num {
			line-height: 50rpx;
			font-size: 30rpx;
		}

		.btns {
			width: 50rpx;
			height: 50rpx;
			border-radius: 50%;
			font-size: 40rpx;
			font-weight: 700;
			color: black;
			text-align: center;
			line-height: 50rpx;
			background-color: #ffd161;
			margin: 0 16rpx;
		}

		.cart {
			position: fixed;
			bottom: 0;
			width: 100%;
			height: 110rpx;
			line-height: 110rpx;
			// background-color: black;
			background-image: linear-gradient(to right, #1F1E1E, #000);
		}

		.cart .cart-container {
			display: flex;
			position: relative;
			color: #eee;
		}

		.cart .del-price-money {
			flex-grow: 1;
			font-size: 28rpx;
			padding-left: 140rpx;
			display: flex;
			flex-direction: column;
		}

		.cart .del-price-money .money {
			font-size: 36rpx;
			height: 60rpx;
			/* line-height: 60rpx; */
			line-height: 110rpx;
		}

		.cart .del-price-money .del-price {
			font-size: 24rpx;
			height: 30rpx;
			line-height: 30rpx;
		}

		.cart .start-del {
			width: 200rpx;
			font-weight: 700;
			font-size: 30rpx;
			text-align: center;
			color: gray;
			/* color: hsla(0,0%,40%,.5); */
			background-image: linear-gradient(-135deg, #fff5ca, #fff2cc);
			/* background-color: #2f2f2f; */
			/* background-color: #17A9EE; */
		}

		.cart .cart-img {
			position: absolute;
			top: -24rpx;
			left: 20rpx;
			width: 100rpx;
			height: 100rpx;
			border-radius: 50%;
			background-color: #666;
			text-align: center;
			z-index: 9;
		}

		.cart .cart-img image {
			width: 60rpx;
			height: 60rpx;
			vertical-align: middle;
			margin-bottom: 20rpx;
		}

		.cart .has-food {
			color: #333;
			background-image: linear-gradient(134deg, #ffde87, #ffc400);
			/* background-color: #ffd161; */
		}

		.cart .cart-list {
			position: absolute;
			bottom: 110rpx;
			background-color: #fff;
			width: 100%;
		}

		.cart .cart-list-header {
			height: 60rpx;
			line-height: 60rpx;
			background-color: #eee;
			text-align: right;
			font-size: 28rpx;
			padding: 0 30rpx;
			color: #333;
		}

		.cart-list-item {
			max-height: 400rpx;
			overflow: scroll;
		}

		.cart-list-item .item {
			display: flex;
			align-items: center;
			margin: 20rpx 0;
		}

		.cart-list-item .item .cart-detail {
			flex: 1;
			/* height: 100rpx; */
			line-height: 40rpx;
			text-align: center;
		}

		.cart-count {
			display: flex;
			align-items: center;
		}

		.bgmain {
			background-color: #ff6344;
			color: white;
		}

		.cuIcon-back {
			color: white;
		}

		.notData {
			display: flex;
			align-items: center;
			flex-direction: column;
			height: 100%;
			justify-content: center;

			.icon {
				width: 130px;
				padding-bottom: 15px;
				filter: grayscale(100);
			}
		}
	}
</style>
