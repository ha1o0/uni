<template>
	<view class="container">
		<view class="top">
			<view class="status-bar"></view>
			<view class="nav-bar">
				<view class="location"><text class="location-text">上海</text><uv-icon name="arrow-down"></uv-icon></view>
				<view class="search"><uv-search placeholder="请输入搜索内容" v-model="keyword" :showAction="false"></uv-search></view>
			</view>
			<view class="extra"></view>
		</view>
		<view class="ads">
			<uv-swiper :list="list" indicator indicatorMode="line" circular @click="onAd"></uv-swiper>
		</view>
		<view class="menus">
			<uv-scroll-list @right="right" @left="left">
				<view class="scroll-list" style="flex-direction: row;">
					<view 
						class="scroll-list__goods-item" 
						v-for="(item, index) in menuList" 
						:key="index" 
						:class="[(index === 9) && 'scroll-list__goods-item--no-margin-right']"
					>
						<image class="scroll-list__goods-item__image" :src="item.image"></image>
						<text class="scroll-list__goods-item__text">￥{{ item.price }}</text>
					</view>
					<view class="scroll-list__show-more">
						<text class="scroll-list__show-more__text">查看更多</text>
					</view>
				</view>
			</uv-scroll-list>
		</view>
		
	</view>
</template>

<script>
	import {
		mapState,
		mapMutations
	} from 'vuex'
	import {
		univerifyLogin
	} from '@/common/univerify.js'

	export default {
		data() {
			return {
				list: [
					'https://cdn.uviewui.com/uview/swiper/swiper3.png',
					'https://cdn.uviewui.com/uview/swiper/swiper2.png',
					'https://cdn.uviewui.com/uview/swiper/swiper1.png',
				],
				menuList: [{
					price: '230.5',
					image: 'https://via.placeholder.com/60x60.png/3c9cff/fff'
				}, {
					price: '74.1',
					image: 'https://via.placeholder.com/60x60.png/f9ae3d/fff'
				}, {
					price: '8457',
					image: 'https://via.placeholder.com/60x60.png/5ac725/fff'
				}, {
					price: '1442',
					image: 'https://via.placeholder.com/60x60.png/f56c6c/fff'
				}, {
					price: '541',
					image: 'https://via.placeholder.com/60x60.png/909399/fff'
				}, {
					price: '234',
					image: 'https://via.placeholder.com/60x60.png/3c9cff/fff'
				}, {
					price: '562',
					image: 'https://via.placeholder.com/60x60.png/f9ae3d/fff'
				}, {
					price: '251.5',
					image: 'https://via.placeholder.com/60x60.png/5ac725/fff'
				}]
			}
		},
		computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		// onLoad() {
		// 	const loginType = uni.getStorageSync('login_type')
		// 	if (loginType === 'local') {
		// 		this.login(uni.getStorageSync('username'))
		// 		return
		// 	}
		// 	let uniIdToken = uni.getStorageSync('uni_id_token')
		// 	if (uniIdToken) {
		// 		this.login(uni.getStorageSync('username'))
		// 		uniCloud.callFunction({
		// 			name: 'user-center',
		// 			data: {
		// 				action: 'checkToken',
		// 			},
		// 			success: (e) => {

		// 				console.log('checkToken success', e);

		// 				if (e.result.code > 0) {
		// 					//token过期或token不合法，重新登录
		// 					if (this.forcedLogin) {
		// 						uni.reLaunch({
		// 							url: '../login/login'
		// 						});
		// 					} else {
		// 						uni.navigateTo({
		// 							url: '../login/login'
		// 						});
		// 					}
		// 				}
		// 			},
		// 			fail(e) {
		// 				uni.showModal({
		// 					content: JSON.stringify(e),
		// 					showCancel: false
		// 				})
		// 			}
		// 		})
		// 	} else {
		// 		this.guideToLogin()
		// 	}
		// },
		methods: {
			...mapMutations(['login']),
			guideToLogin() {
				uni.showModal({
					title: '未登录',
					content: '您未登录，需要登录后才能继续',
					/**
					 * 如果需要强制登录，不显示取消按钮
					 */
					showCancel: !this.forcedLogin,
					success: (res) => {
						if (res.confirm) {
							univerifyLogin().catch((err) => {
								if (err === false) return;
								/**
								 * 如果需要强制登录，使用reLaunch方式
								 */
								if (this.forcedLogin) {
									uni.reLaunch({
										url: '../login/login'
									});
								} else {
									uni.navigateTo({
										url: '../login/login'
									});
								}
							})
						}
					}
				});
			},
			onAd(index) {
				console.log(index)
			},
			left() {
				console.log('left')
			},
			right() {
				console.log('right')
			},
		}

	}
</script>

<style lang="scss" scoped>
@mixin flex($direction: row) {
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	flex-direction: $direction;
}
.container {
	@include flex(column);
	background: #fff;
	width: 100%;
	height: 100vh;
	padding: 0 10px;

	> view {
		width: calc(100% - 20px);
		margin-bottom: 15px;
	}

	.top {
		@include flex(column);
		height: calc(var(--status-bar-height) + 44px);
		position: sticky;
		top: 0;
		padding: 0;
		margin-bottom: 0;
		
		.status-bar {
			height: var(--status-bar-height);
		}

		.nav-bar {
			@include flex();
			align-items: center;
			height: 32px;
			.location {
				@include flex();
				margin-right: 30px;

				.location-text {
					margin-right: 3px;
				}
			}
		}

		.extra {
			height: 0px;
		}

		.search {
			width: 350rpx;
		}
	}

	.menus {
		.scroll-list {
			@include flex(column);
			&__goods-item {
				margin-right: 20px;
				&__image {
					width: 60px;
					height: 60px;
					border-radius: 4px;
				}
				&__text {
					color: #f56c6c;
					text-align: center;
					font-size: 12px;
					margin-top: 5px;
				}
			}
			&__show-more {
				background-color: #fff0f0;
				border-radius: 3px;
				padding: 3px 6px;
				@include flex(column);
				align-items: center;
				&__text {
					font-size: 12px;
					width: 12px;
					color: #f56c6c;
					line-height: 16px;
				}
			}
		}
	}

	
}
	
	
</style>
