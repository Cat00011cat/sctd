<template>
	<view>
		身份证ID：
		{{data}}
		<view class="myBg">
			<image src="../../static/icon/allservices.png" mode="" class="myAvatar"></image>
			<text class="myName">NAME</text>
		</view>
		
		<view class="myMenu">
			
			<view class="myTitle">
				<text>个人信息</text>
				<text class="right">></text>
			</view>
			
			
			
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: '',

			}
		},
		onLoad() {
			this.a();
		},
		methods: {
			a() {
				var str = "76524367827635287163";
				var data = str.slice(0, 2) + "************" + str.slice(14, 18);
				this.data = data;
			}
		}
	}
</script>

<style>
	
</style>
