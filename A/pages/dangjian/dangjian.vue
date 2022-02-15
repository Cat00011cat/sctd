<template>
	<view>
		<!-- 轮播党建展示 -->
		<view class="swiperBox">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiperBox">
				<swiper-item>
					<view class="swiper-item">
						<image class="banner"
							src="http://www.ynftc.cn/upload/main/advertisement/b7dbc382c29348eabf98e934b303c4dc_1920_500.jpg">
						</image>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image class="banner"
							src="http://www.ynftc.cn/upload/main/advertisement/bbae7f12c2594d0092c2983c5179b1b1_1920_500.jpg">
						</image>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image class="banner"
							src="http://www.ynftc.cn/upload/main/advertisement/337d889a5c2040b097a2d17539a90f5b_1920_500.jpg">
						</image>
					</view>
				</swiper-item>
			</swiper>
		</view>


		<!-- 党建大厅各个服务入口 -->
		<view>
			<view class="serviceBox">
				<block v-for="(item,index) in service" :key="index">
					<view class="service-top-item" @click="toService(index)">
						<image :src="item.img" class="service-top-item-icon"></image>
						<view class="service-top-item-icon-desc">{{item.name}}</view>
					</view>
				</block>
			</view>
		</view>


		<!-- 党员动态 这个资讯就不单独写一个按钮了，，直接渲染页撑起来。 -->
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
				service: [{
					"name": "党建动态",
					"img": "../../static/icon3/djdt.png"
				}, {
					"name": "党员学习",
					"img": "../../static/icon3/dyxx.png"
				}, {
					"name": "组织活动",
					"img": "../../static/icon3/djhd.png"
				}, {
					"name": "建言献策",
					"img": "../../static/icon3/jyxc.png"
				}, {
					"name": "随手拍",
					"img": "../../static/icon3/ssp.png"
				}],
				tabList: [], //新闻分类
				newDataList: [], //新闻列表
				type: 0,


			}
		},
		onShow() {
			this.getAllNewsTab();
		},
		methods: {
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
			getImgUrl(item) {
				return urlHead + item;
			}

		}
	}
</script>

<style>
	.swiperBox {
		margin: 15rpx;
		height: 150px;
		border-radius: 20rpx;
	}

	.banner {
		width: 720rpx;
		height: 150px;
		border-radius: 20px;
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

	.newbox {
		margin: 280rpx 15rpx 15rpx 15rpx;
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
		color: #ff0000;
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
