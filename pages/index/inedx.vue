<!-- 首页 -->
<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="false">
			<!-- <block slot="backText">返回</block> -->
			<block slot="content">首页</block>
		</cu-custom>

		<add-tip :tip="tip" :duration="duration" />

		<!-- 流量主-腾讯广告 -->
		<ad unit-id="adunit-961458988ac9ad8b" ad-intervals="30"></ad>

		<!-- 导航栏 -->
		<view class="cu-list grid solids-bottom col-4 no-border">
			<view class="cu-item" v-for="(item, index) in categories" :key="index"
				:style="[{ animation: 'show ' + ((index + 1) * 0.2 + 1) + 's 1' }]" @click="goCategorieslist"
				:data-mid="item.mid">
				<view :class="['cuIcon-' + item.cuIcon, 'text-' + item.color]">
					<view class="cu-tag badge" v-if="item.count != 0">
						<block v-if="item.badge != 1">{{ item.badge > 99 ? '99+' : item.badge }}</block>
					</view>
				</view>
				<text>{{ item.name }}</text>
			</view>
		</view>

		<view class="message-box">
			<view class="page-section swiper">
				<view class="page-section-spacing">
					<swiper style="height: 120rpx;" class="swiper" vertical="ture" circular="true"
						indicator-dots='false' indicator-color="rgba(0,0,0,.0)" indicator-active-color="rgba(0,0,0,.0)"
						autoplay="true" interval="4000">
						<swiper-item class="swiper-list" v-for="(item, index) in messageData" :key="index">
							<view class="message-tltle">{{item.title}}</view>
							<view class="message-content"><span>{{item.tag}}</span>{{item.content}}</view>
						</swiper-item>
					</swiper>
				</view>
			</view>
		</view>

		<view class="cu-bar bg-white margin-top-xs">
			<view class="action sub-title">
				<text class="text-xl text-bold text-blue text-shadow">热门视频</text>
				<text class="text-ABC text-blue">curriculum</text>
			</view>
			<view class="action" @click="goVideo"><text class="text-lg text-grey text-shadow">更多</text></view>
		</view>

		<view class="skill-sequence-panel-content-wrapper">
			<!--左边虚化-->
			<view class="hide-content-box hide-content-box-left"></view>
			<!--右边虚化-->
			<view class="hide-content-box hide-content-box-right"></view>
			<scroll-view scroll-x="true" class="kite-classify-scroll">
				<view class="kite-classify-cell shadow" v-for="(item, index) in curriculum" :key="index">
					<view :class="'nav-li bg-index' + (index + 1)">
						<view class="nav-name">{{ item.name }}</view>
					</view>
					<view class="nav-content">{{ item.content }}</view>
					<view @click="goVideo" class="nav-btn shadow" :class="'bg-index' + (index + 1)">立即学习</view>
				</view>
			</scroll-view>
		</view>
		<view class="cu-bar bg-white margin-top-xs">
			<view class="action sub-title">
				<text class="text-xl text-bold text-blue text-shadow">路由器测试</text>
				<text class="text-ABC text-blue">curriculum</text>
			</view>
			<view class="action" @click="goProjectList"><text class="text-lg text-grey text-shadow">更多</text></view>
		</view>
		<view style="height: 140rpx;width: 1rpx;"></view>
	</view>
</template>

<script>
	import request from '@/common/request.js';
	import addTip from '../../components/wxcomponents/struggler-uniapp-add-tip/struggler-uniapp-add-tip.vue';
	export default {
		components: {
			addTip
		},
		data() {
			return {
				tip: '点击「添加小程序」，下次访问更便捷',
				duration: 1,

				scrollTop: 0,
				old: {
					scrollTop: 0
				},

				bannerList: [{
						imageUrl: 'https://cdn.zhoukaiwen.com/zjx_banner.png'
					},
					{
						imageUrl: 'https://cdn.zhoukaiwen.com/zjx_banner3.png'
					},
					{
						imageUrl: 'https://cdn.zhoukaiwen.com/zjx_banner1.png'
					},
					{
						imageUrl: 'https://cdn.zhoukaiwen.com/zjx_banner2.png'
					}
				],
				categories: [{
						cuIcon: 'hotfill',
						color: 'red',
						badge: 1,
						mid: '1',
						name: '系统状态'
					},
					{
						cuIcon: 'colorlens',
						color: 'orange',
						badge: 1,
						mid: '2',
						name: '高级设置'
					},
					{
						cuIcon: 'goodsnewfill',
						color: 'yellow',
						badge: 1,
						mid: '3',
						name: '无线设置'
					},
					{
						cuIcon: 'calendar',
						color: 'cyan',
						badge: 1,
						mid: '4',
						name: '海洋实验室通信资讯'
					}
				],
				messageData: [{
					title: '定制版更多功能，敬请期待！',
					tag: '更新',
					content: '致力提供优质的app'
				}],
				curriculum: [{
						name: '项目开发',
						content: ''
					},
					{
						name: '项目开发',
						content: ''
					},
					{
						name: '项目开发',
						content: ''
					},
					{
						name: '项目开发',
						content: ''
					},
					{
						name: '项目开发',
						content: ''
					}
				],
				projectList: []
			};
		},
		watch: {},
		mounted() {
			console.log(this.projectList);
			this.getData();
		},
		methods: {
			getData() {
				console.log('数据加载');
				let opts = {
					url: 'json/project.json',
					method: 'get'
				};
				uni.showLoading({
					title: '加载中'
				});
				request.httpRequest(opts).then(res => {
					console.log(res);
					uni.hideLoading();
					if (res.statusCode == 200) {
						this.projectList = res.data.data;
					} else {}
				});
			},
			scroll: function(e) {
				console.log(e);
				this.old.scrollTop = e.detail.scrollTop;
			},
			goCategorieslist: function(e) {
				console.log(e.currentTarget.dataset.mid)
				if (e.currentTarget.dataset.mid == 1)
				{
					uni.navigateTo({
						url: '../timeline'
					});
				}
				else if(e.currentTarget.dataset.mid == 2)
				{
					uni.navigateTo({
						url:'../Advanced_settings'
					})
				}
				else if (e.currentTarget.dataset.mid == 3) {
					uni.navigateTo({
						url: '../../os_project/index'
					});
				} else if (e.currentTarget.dataset.mid == 4) {
					this.$emit('ShowNews', 'news')
					console.log('文章资讯')

				}
			},
			goProjectList() {
				uni.navigateTo({
					url: '../project/list'
				});
			},
			goProject(id) {
				uni.navigateTo({
					url: '../project/project?proId=' + id
				});
			},
			goVideo() {
				uni.navigateTo({
					url: '../video'
				});
			}
		}
	};
</script>
<style lang="scss" scoped>
	.swiper-box {
		flex: 1;
	}

	.swiper-item {
		height: 100%;
	}

	.message-box {
		width: 100%;
		height: 120rpx;
		background: url(https://zhoukaiwen.com/img/icon/clock.gif) #FFFFFF;
		background-repeat: no-repeat;
		background-size: 100rpx 100rpx;
		background-position: 15rpx 10rpx;
		margin: 0rpx 0rpx 10rpx 0rpx;
		padding-left: 130rpx;

		.message-tltle {
			height: 65rpx;
			line-height: 70rpx;
			font-weight: 600;
			font-size: 28rpx;
		}

		.message-content {
			color: #0081ff;

			span {
				background-color: #0081ff;
				color: #FFFFFF;
				padding: 2rpx 8rpx;
				border-radius: 8rpx;
				margin-right: 8rpx;
			}
		}
	}

	/*scroll-view外层*/
	.skill-sequence-panel-content-wrapper {
		position: relative;
		white-space: nowrap;
		padding: 10rpx 0 10rpx 10rpx;
	}

	/*左右渐变遮罩*/
	.hide-content-box {
		position: absolute;
		top: 0;
		height: 100%;
		width: 10px;
		z-index: 2;
	}

	.hide-content-box-left {
		left: 0;
		background-image: linear-gradient(to left, rgba(255, 255, 255, 0), #f3f3f3 60%);
	}

	.hide-content-box-right {
		right: 0;
		background-image: linear-gradient(to right, rgba(255, 255, 255, 0), #f3f3f3 60%);
	}

	.kite-classify-scroll {
		width: 100%;
		height: 380rpx;
		overflow: hidden;
		white-space: nowrap;
	}

	.kite-classify-cell {
		display: inline-block;
		width: 266rpx;
		height: 370rpx;
		margin-right: 20rpx;
		background-color: #ffffff;
		border-radius: 10rpx;
		overflow: hidden;
		box-shadow: 2px 2px 3px rgba(26, 26, 26, 0.2);
	}

	.nav-li {
		padding: 40rpx 30rpx;
		width: 100%;
		background-image: url(https://s1.ax1x.com/2020/06/27/NyU04x.png);
		background-size: cover;
		background-position: center;
		position: relative;
		z-index: 1;
	}

	.nav-name {
		font-size: 28upx;
		text-transform: Capitalize;
		margin-top: 20upx;
		position: relative;
	}

	.nav-name::before {
		content: '';
		position: absolute;
		display: block;
		width: 40rpx;
		height: 6rpx;
		background: #fff;
		bottom: 0;
		right: 0;
		opacity: 0.5;
	}

	.nav-name::after {
		content: '';
		position: absolute;
		display: block;
		width: 100rpx;
		height: 1px;
		background: #fff;
		bottom: 0;
		right: 40rpx;
		opacity: 0.3;
	}

	.nav-content {
		width: 100%;
		padding: 15rpx;
		display: inline-block;
		overflow-wrap: break-word;
		white-space: normal;
	}

	.nav-btn {
		width: 200rpx;
		height: 60rpx;
		margin: 8rpx auto;
		text-align: center;
		line-height: 60rpx;
		border-radius: 10rpx;
	}

	.bg-index1 {
		background-color: #19cf8a;
		color: #fff;
	}

	.bg-index2 {
		background-color: #954ff6;
		color: #fff;
	}

	.bg-index3 {
		background-color: #5177ee;
		color: #fff;
	}

	.bg-index4 {
		background-color: #f49a02;
		color: #fff;
	}

	.bg-index5 {
		background-color: #ff4f94;
		color: #fff;
	}

	.bg-index6 {
		background-color: #7fd02b;
		color: #fff;
	}

	.item-name {
		margin-bottom: 15rpx;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1;
		overflow: hidden;
	}
</style>
