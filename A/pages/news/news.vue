<template>
	<view>
		<!-- 轮播图 点击跳转新闻 -->
		<!-- 新闻分类  展示各类新闻主题-->
		<!-- 新闻列表 -->
		<!-- 详情页 添加新闻推荐-->

		<view>

			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiperBox">
				<block v-for="(item,index) in banner" :key="index">
					<swiper-item>
						<image class="banner" :src="getImgUrl(item.cover)" />
					</swiper-item>
				</block>
			</swiper>
		</view>

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
							<view class="time">👁 观看数：{{item.readNum>0 ? item.readNum : 0}}</view>
							<view class="time">♥ 点赞数：{{item.likeNum>0 ? item.likeNum : 0}}</view>
							<view class="time">⏰ 发布时间：{{item.publishDate}}</view>
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
				newDataList: [], //新闻列表
				banner: [], //存储新闻图片banner
				tabList: [], //存储分类
				type: 0, //新闻分类id
			}
		},
		onShow() {
			this.getBanner();
			this.getAllNewsTab();
		},
		methods: {
			getBanner() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/press/press/list",
					method: 'GET',
					success(res) {
						var data = res.data;
						that.banner = data.rows.length > 3 ? data.rows.slice(0, 3) : data.rows;
						console.log(that.banner);

					}
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
		border-radius: 20rpx;
	}

	/*  */
	.newbox {
		margin: 50rpx 15rpx 15rpx 15rpx;
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
