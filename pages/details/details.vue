<template>
	<view>
		<view class="PreviewImage" v-if="showPreviewImage === true">
			<view class="PreviewImage_dv1" @click="nonePreviewImage">

			</view>
			<image :src="itemDetailsData.poster" mode="" class="PreviewImage_img" @longpress="longpress"></image>
			<view class="PreviewImage_dv2" @click="nonePreviewImage">

			</view>
		</view>
		<view class="" v-else>
			<view class="details_first">
			<!-- 	<view class="first_return">
					<image src="../../static/picture/return.png" mode="" class="first_return_img" @click="returnObj"></image>
				</view>
				<view class="first_share">
					<image src="../../static/picture/share.png" mode="" class="first_share_img"></image>
				</view> -->
				<video :src="itemDetailsData.trailer" controls :poster="itemDetailsData.poster" class="videoObj_details" :id="itemDetailsData.id"
				 @play="playObj"></video>
			</view>
			<view class="details_second">
				<view class="details_second_left" @click="previewImage">
					<image :src="itemDetailsData.poster" mode="" class="second_left_img"></image>
				</view>
				<view class="details_second_right">
					<view class="second_right_name">{{itemDetailsData.name}}</view>
					<view class="second_right_basicInfo">{{itemDetailsData.basicInfo}}</view>
					<view class="second_right_basicInfo">{{itemDetailsData.originalName}}</view>
					<view class="second_right_basicInfo">{{itemDetailsData.releaseDate}}</view>
					<view class="second_right_last">
						<view class="right_last_1">
							<view class="right_last_1_1">综合评分：</view>
							<view class="right_last_1_2">{{itemDetailsData.Rate}}</view>
						</view>
						<view class="right_last_2">
							<view class="scroll_item_rate">
								<uni-rate :value="itemDetailsData.Rate" :disabled="disabledBle" size="12"></uni-rate>
							</view>
							<view class="right_last_2_2">人点赞</view>
						</view>
					</view>
				</view>
			</view>
			<view class="details_third">
				<view class="details_third_plot">剧情介绍：</view>
				{{itemDetailsData.plotDesc}}
			</view>
			<view class="details_fourth">
				<view class="details_third_plot">演职人员：</view>
				<scroll-view class="home_second_scroll" scroll-x="true" scroll-left="120">
					<view class="second_scroll_dv">
						<view class="second_scroll_item" v-for="(item,index) in directorData" :key="item.staffId">
							<image :src="item.photo" mode="" class="scroll_item_img"></image>
							<view class="scroll_item_name">{{item.name}}</view>
							<view class="scroll_item_actName">{{item.actName}}</view>
						</view>
						<view class="second_scroll_item" v-for="(item,index) in actorsData" :key="index">
							<image :src="item.photo" mode="" class="scroll_item_img"></image>
							<view class="scroll_item_name">{{item.name}}</view>
							<view class="scroll_item_actName">饰演:{{item.actName}}</view>
						</view>
					</view>
				</scroll-view>
			</view>
			<view class="details_fifth">
				<view class="details_third_plot">剧照：</view>
				<view class="details_fifth_item">
					<view class="fifth_item_Div" v-for="(item,index) in stillData" :key="index">
						<image :src="item" mode="" class="fifth_item_img"></image>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from "../../components/uni-ui/uni-rate/uni-rate.vue"
	export default {
		components: {
			uniRate
		},
		data() {
			return {
				itemDetailsData: {},
				disabledBle: true,
				actorsData: [], //演职人员
				directorData: [], //导演
				stillData: [], //剧照
				showPreviewImage: false
			}
		},
		//监听页面隐藏
		onHide: function() {

		},
		//用户点击右上角分享
		 onShareAppMessage: function () {
		     return {
				 title: this.itemDetailsData.name,
				 path: `/pages/details/details`,
				 success: (res) => {
					 console.log(res);
					 console.log(222);
				 },
				 fail: (err) => {
					 console.log(err);
				 }
			 }
		 },
		onLoad(options) {
			this.itemDetailsData = JSON.parse(options.itemId)
			this.stillData = JSON.parse(this.itemDetailsData.plotPics)
			console.log(this.itemDetailsData);
			console.log(this.stillData);
			this.getTrailerData()
			this.getDirectorData()
		},
		methods: {
			playObj() {

			},
			//获取演职人员
			getTrailerData() {
				uni.request({
					url: `${this.$api}/search/staff/${this.itemDetailsData.id}/2?qq=434714873`,
					method: 'POST',
					data: {},
					success: res => {
						this.actorsData = res.data.data
						// console.log(this.actorsData);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取导演
			getDirectorData() {
				uni.request({
					url: `${this.$api}/search/staff/${this.itemDetailsData.id}/1?qq=434714873`,
					method: 'POST',
					data: {},
					success: res => {
						this.directorData = res.data.data
						console.log(this.directorData);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//返回
			returnObj() {
				uni.switchTab({
					url: `../home/home`
				})
			},
			//图片预览
			previewImage() {
				this.showPreviewImage = true
			},
			//取消图片预览
			nonePreviewImage() {
				this.showPreviewImage = false
			},
			//长按图片事件
			longpress() {
				uni.showActionSheet({
					itemList: ['保存到本地图片'],
					success: res => {
						uni.downloadFile({
							url: this.itemDetailsData.poster,
							success(res) {
								console.log(res);
								if (res.statusCode === 200) {
									uni.saveImageToPhotosAlbum({
										filePath: res.tempFilePath,
										success: res => {
											console.log(res)
											uni.showToast({
												title: '保存成功'
											})
										},
										fail: err => {
											console.log(err);
											uni.showToast({
												title: '保存失败，请稍后重试',
												icon: 'none'
											})
										}
									})
								}
							}
						})
					},
					fail: function(res) {
						console.log(res.errMsg);
					}
				});
			}
		}
	}
</script>

<style>
	.PreviewImage {
		width: 100%;
		height: 100%;
	}

	.PreviewImage_dv1 {
		width: 100%;
		height: 420rpx;
		background-color: #000000;
	}

	.PreviewImage_dv2 {
		width: 100%;
		height: 420rpx;
		background-color: #000000;
		margin-top: -12rpx;
	}

	.PreviewImage_img {
		width: 100%;
	}

	.PreviewImage_popop {
		z-index: 10000;
		color: #F7F7F7;
	}

	.details_first {
		position: relative;
	}

	.first_return,
	.first_share {
		width: 60rpx;
		height: 60rpx;
		display: flex;
		align-items: center;
		background-color: #515151;
		border-radius: 50%;
	}

	.first_return {
		position: absolute;
		top: 10rpx;
		left: 10rpx;
		z-index: 1000;
	}

	.first_share {
		position: absolute;
		top: 10rpx;
		right: 10rpx;
		z-index: 1000;
	}

	.first_return_img {
		width: 40rpx;
		height: 40rpx;
		margin-left: 50%;
		transform: translateX(-20rpx)
	}

	.first_share_img {
		width: 50rpx;
		height: 40rpx;
		margin-left: 50%;
		transform: translateX(-15rpx)
	}

	.videoObj_details {
		width: 100%;
		height: 400rpx;
	}

	.details_second {
		width: 90%;
		margin: 0 auto;
		padding: 60rpx 20rpx;
		display: flex;
		justify-content: space-between;
		border-bottom: 4rpx solid #C8C8C8;
	}

	.second_left_img {
		width: 280rpx;
		height: 320rpx;
	}

	.details_second_right {
		margin-left: 10rpx;
	}

	.second_right_name {
		font-size: 40rpx;
		font-weight: bold;
	}

	.second_right_basicInfo {
		font-size: 30rpx;
		color: #808080;
	}

	.second_right_last {
		display: flex;
		justify-content: space-between;
	}

	.right_last_1 {
		text-align: center;
	}

	.right_last_1 {
		font-size: 40rpx;
	}

	.right_last_1_2 {
		color: #C4BD36;
	}

	.right_last_2_2 {
		font-size: 30rpx;
		color: #808080;
	}

	.details_third {
		padding: 20rpx;
		font-size: 35rpx;
	}

	.details_third_plot {
		font-size: 30rpx;
		color: #AAAAAA;
	}

	.details_fourth {
		width: 90%;
		margin: 0 auto;
		margin-top: 10rpx;
		padding: 10rpx;
		border-top: 4rpx solid #C8C8C8;
	}

	.home_second_scroll {
		width: 100%;
		/* padding-right: 20rpx; */
	}

	.second_scroll_dv {
		display: flex;
		flex-wrap: nowrap;
	}

	.second_scroll_item {
		width: 200rpx;
		text-align: center;
		margin-left: 20rpx;
	}

	.scroll_item_img {
		width: 180rpx;
		height: 180rpx;
	}

	.scroll_item_name {
		width: 180rpx;
		font-size: 30rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.scroll_item_actName {
		width: 180rpx;
		font-size: 30rpx;
		color: #808080;
	}

	.details_fifth {
		width: 90%;
		margin: 0 auto;
		margin-top: 10rpx;
		padding: 10rpx;
		border-top: 4rpx solid #C8C8C8;
	}

	.details_fifth_item {
		display: flex;
		flex-wrap: wrap;
	}

	.fifth_item_img {
		width: 180rpx;
		height: 180rpx;
		margin-left: 10rpx;
	}
</style>
