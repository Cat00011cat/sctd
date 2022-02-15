<template>
	<view>

		<!-- 		<view class="pric">
			<scroll-view scroll-y="true" class="leftView">
				<view v-for="(item,index) in leftList" :key="index" class="a">{{item}}</view>
			</scroll-view>
			<scroll-view scroll-y="true" class="rightView">
				<view class="a">111</view>
			</scroll-view>
		</view> -->


		<view class="serviceBox">
			<block v-for="(item,index) in serviceList" :key="index">
				<view class="service-top-item" @click="toService(index)">
					<image :src="getImgUrl(item.imgUrl)" class="service-top-item-icon"></image>
					<view class="service-top-item-icon-desc">{{item.serviceName}}</view>
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
				serviceList: [], //服务
				leftList: ['车主服务', '生活服务', '便民服务']
			}
		},
		onLoad() {
			this.getServiceList();
		},
		methods: {
			getServiceList() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/api/service/list",
					success(res) {
						var data = res.data;
						console.log(res.data)
						that.serviceList = data.rows;
					}
				})
			},
			toService(index) {
				console.log(index)
				var ulink = this.serviceList[index].link;
				console.log(ulink);
				uni.navigateTo({
					url: ulink,
				})
			},
			getImgUrl(index) {
				return urlHead + index;
			}
		}
	}
</script>

<style>
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

	/* test*/
	/* 	.pric {
		height: 100%;
		display: flex;
	}

	.leftView {
		width: 300rpx;
		height: 100%;
		border-right: 1rpx solid #000000;

	}

	.a {
		padding: 10px
	} */
</style>
