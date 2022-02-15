<template>
	<view>
		<!-- 搜索 -->
		<view>
			<view class="searchBox">
				<view class="searchBg">
					<input placeholder="输入内容搜索" v-model="searchValue" confirm-type="search"
						@confirm="doSearch(searchValue)" />
				</view>
			</view>
		</view>

		<!-- 轮播 -->
		<view>
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiperBox">
				<block v-for="(item,index) in bannerList" :key="index">
					<swiper-item>
						<image class="banner" :src="getImgUrl(item.advImg)" @click="bannerXq(index)" />
					</swiper-item>
				</block>
			</swiper>
		</view>

		<!-- 服务 -->
		<view>
			<view class="serviceBox">
				<block v-for="(item,index) in serviceList" :key="index">
					<view class="service-top-item" @click="toService(index)">
						<image :src="getImgUrl(item.imgUrl)" class="service-top-item-icon"></image>
						<view class="service-top-item-icon-desc">{{item.serviceName}}</view>
					</view>
				</block>
				<view class="service-top-item" @click="toAllservice()">
					<image src="../../static/icon/selcollect.png" class="service-top-item-icon"></image>
					<view class="service-top-item-icon-desc">NAME</view>
				</view>
			</view>
		</view>

		<!-- 主题 -->
		<view>
			<view class="Theme">
				<block v-for="(item,index) in hotTheme" :key="index">
					<view class="Theme-top-item" v-if="item.hot == 'Y'" @click="getNewItem(index)">
						<image class="Theme-top-item-icon" :src="getImgUrl(item.cover)"></image>
						<view class="Theme-top-item-icon-desc">{{item.title.slice(0,8)+'……'}}</view>
					</view>
				</block>
			</view>
		</view>

		<!-- 新闻 -->
		<view>
			<view class="newbox">
				<scroll-view class="scroll-x" scroll-x="true">
					<block v-for="(item, index) in tabList" :key="index">
						<text class="title" @click="choose(item.id)" v-if="item.id != type">{{item.name}}</text>
						<text class="title itemTabSelect" @click="choose(item.id)" v-else>{{item.name}}</text>
					</block>
				</scroll-view>

				<block v-for="(item,index) in newDataList" :key="item.id">
					<view class="bg" @click="getNewItem(item.id)">
						<view class="newtitle">{{item.title}}
							<image :src="getImgUrl(item.cover)" class="newimg"></image>
						</view>
						<view>
							<rich-text class="desc"
								:nodes="item.content.length>40 ? item.content.slice(0,40)+'……' : item.content">
							</rich-text>
						</view>
						<view>
							<text class="time">评论总数：{{item.commentNum>0 ? item.commentNum : 0}}</text>
						</view>
						<view>
							<text class="time">发布时间：{{item.publishDate}}</text>
						</view>
					</view>
				</block>
			</view>
		</view>


	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				type: 0, //新闻分类id
				searchValue: '', //搜索值
				bannerList: [], //banner
				serviceList: [], //服务
				tabList: [], //新闻分类
				newDataList: [], //新闻列表
				hotTheme: [], //热门主题
			}
		},
		onLoad() {
			this.getBannerList();
			this.getServiceList();
			this.getTheme();
		},
		onShow() {
			this.getAllNewsTab();
		},
		methods: {
			doSearch(searchValue) {
				console.log(searchValue)
			},
			getBannerList() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/api/rotation/list?type=2",
					method: "GET",
					success(res) {
						var data = res.data;
						console.log(res.data)
						that.bannerList = data.rows;
					}
				})
			},
			bannerXq(index) {
				switch (index) {
					case 0:
						console.log('点击了0');
						break;
					case 1:
						console.log('点击了1');
						break;
					case 2:
						console.log('点击了2')
						break;
					default:
						console.log('没有获取到任何参数')
				}
			},
			getServiceList() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/api/service/list",
					success(res) {
						var data = res.data;
						console.log(res.data)
						that.serviceList = data.rows.length > 9 ? data.rows.slice(0, 9) : data.rows;
					}
				})
			},
			toService(index) {
				var ulink = this.serviceList[index].link;
				console.log(ulink)
				uni.navigateTo({
					url: ulink,
				})
			},
			toAllservice() {
				uni.switchTab({
					url: '../allService/allService'
				})
			},
			getAllNewsTab() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/press/category/list",
					method: 'GET',
					success(res) {
						var data = res.data;
						console.log(data);
						that.tabList = data.data;
						that.type = that.tabList[0].id;
						that.getNewList(that.type);
					}
				})
			},
			choose(index) {
				this.type = index;
				this.getNewList(this.type);
			},
			getNewList(type) {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/press/press/list?type=" + type,
					method: 'GET',
					success(res) {
						var data = res.data;
						console.log(data);
						that.newDataList = data.rows;
					}
				})
			},
			getTheme() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/press/press/list",
					method: 'GET',
					success(res) {
						var data = res.data;
						console.log(data);
						that.hotTheme = data.rows;
					}
				})
			},
			getNewItem(e) {
				uni.navigateTo({
					url: 'newsss?id=' + e,
					success() {
						console.log(e);
					}
				})
			},
			getImgUrl(item) {
				return urlHead + item;
			}

		}
	}
</script>

<style>
	.searchBox {
		padding: 15rpx;
	}

	.searchBg {
		background-color: #ebeaef;
		border-radius: 15rpx;
		height: 60rpx;
	}

	.swiperBox {
		margin: 15rpx;
		height: 150px;
		border-radius: 20rpx;
	}

	.banner {
		width: 720rpx;
		height: 150px;
		border-radius: 20rpx;
	}

	.service {
		margin: 15rpx;
		border-radius: 15rpx;
	}

	.service-top-item {
		float: left;
		margin: 10px 0px 0px 0px;
		width: 20%;
	}

	.service-top-item-icon {
		width: 50px;
		height: 50px;
		margin: 0 auto;
		padding: 10px;
	}

	.service-top-item-icon-desc {
		text-align: center;
	}

	.Theme {
		float: left;
		margin: 15rpx;
		width: 720rpx;
		border-radius: 15rpx;
		/* background-color: #007AFF; */
	}

	.Theme-top-item {
		float: left;
		margin: 10px 0px 0px 0px;
		width: 50%;
	}

	.Theme-top-item-icon {
		width: 355rpx;
		height: 200rpx;
		background-color: #C8C7CC;
		border-radius: 20px;
	}

	.Theme-top-item-icon-desc {
		text-align: left;
		margin-left: 20rpx;
		margin: 10rpx;
	}

	.newbox {
		margin: 230rpx 15rpx 15rpx 15rpx;
	}

	.scroll-x {
		display: flex;
		white-space: nowrap;
	}

	.title {
		padding: 0 30rpx;
		font-size: 18px;
	}

	.itemTabSelect {
		color: #007AFF;
		border-bottom-width: 3px;
	}

	.bg {
		margin: 15rpx;
		border-radius: 5px;
		/* background-color: #C8C7CC; */
		padding: 10px;
	}

	.newtitle {
		font-size: 18px;
		font-weight: bold;
		/* 行高 */
		line-height: 30px;
	}

	.newimg {
		margin-top: 5px;
		width: 100%;
		height: 150px;
		border-radius: 5px;
	}

	.desc {
		margin-top: 5px;
		font-size: 16px;
		line-height: 30px;
	}

	.time {
		margin-top: 5px;
		font-size: 14px;
		color: #000000;
	}
</style>
