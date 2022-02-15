<template>
	<view>
		<view class="bg">
			<view class="newtitle">{{newData.title}}</view>
			<view>
				<text class="time">{{newData.publishDate}}</text>
				<button size="time" @click="go()" style="left: 15rpx;">去评论</button>
			</view>
			<image :src="getImgUrl(newData.cover)" class="newimg"></image>
			<rich-text :nodes="newData.content" class="desc" />
		</view>
		<!-- 发表留言 -->
		<view>
			<textarea placeholder="请输入评论"
				style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;border-radius: 15rpx;"></textarea>
			<button>评论</button>
			<!-- 评论列表 -->
			<view>评论条数{{}}</view>
			<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;height: 50rpx;border-radius: 10rpx;">
				测试评论
			</view>
			<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;height: 50rpx;border-radius: 10rpx;">
				测试1111111</view>
			<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;border-radius: 10rpx;">
				测试2222222222222222222222221111111111111111111111笑死我了</view>
		</view>

		<!-- 推荐新闻 -->
		<view>
			<block v-for="(item,index) in newDataList" :key="item.id" v-if="item.top=='Y'">
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
						<text class="time">观看数：{{item.readNum>0 ? item.readNum : 0}}</text>
					</view>
					<view>
						<text class="time">发布时间：{{item.publishDate}}</text>
					</view>
				</view>
			</block>
		</view>

	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				id: 29,
				newData: [],
				newDataList: [], //新闻列表
			}
		},
		onLoad(e) {
			if (e.id != null) {
				this.id = e.id;
			}
			this.getnewItem()
			this.getNewComment()
		},
		onShow() {
			this.getNewList();
		},
		methods: {
			go() {
				uni.pageScrollTo({
					scrollTop: 11000,
					duration: 300,
				})
			},
			getImgUrl(index) {
				return urlHead + index
			},
			getnewItem() {
				let that = this
				uni.request({
					url: urlHead + "/prod-api/press/press/" + that.id,
					method: "GET",
					success(res) {
						that.newData = res.data.data
					}
				})
			},
			getNewList() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/press/press/list",
					method: 'GET',
					success(res) {
						var data = res.data;
						that.newDataList = data.rows;
						console.log(that.newDataList);
					}
				})
			},
		}
	}
</script>

<style>
	/* 新闻内容 */
	/* 模块背景用这个 fefefe   ios简约标准*/
	.bg {
		border-radius: 5px;
		background: #fefefe;
		padding: 10px;
	}

	.newtitle {
		font-size: 18px;
		font-weight: bold;
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
		color: #a6a6a6;
	}
</style>
