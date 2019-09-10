<template>
	<view class="">
		<view class="personal_hide" v-if="showHide === true">
			<view class="personal_hide_dv1" @click="smallImg"></view>
			<image :src="personalDataObj.faceImage" mode="" class="personal_hide_img" @click="smallImg"></image>
			<view class="personal_hide_dv2" @click="smallImg"></view>
		</view>
		<view class="personal_all" v-else>
			<view class="personal_first">
				<view class="personal_first_left">
					头像
				</view>
				<view class="personal_first_right" @click="upLoadAva">
					<image :src="personalDataObj.faceImage" mode="" class="first_right_img1"></image>
					<image src="../../static/picture/right.png" mode="" class="first_right_img2"></image>
				</view>
			</view>
			<view class="personal_first">
				<view class="personal_first_left">
					昵称
				</view>
				<view class="personal_second_right">
				     {{personalDataObj.username}}
					 <image src="../../static/picture/right.png" mode="" class="first_right_img2"></image>
				</view>
			</view>
			<view class="personal_first">
				<view class="personal_first_left">
					生日
				</view>
				<view class="personal_second_right" @click="jumpSetBirthday">
				     {{personalDataObj.birthday}}
					 <image src="../../static/picture/right.png" mode="" class="first_right_img2"></image>
				</view>
			</view>
			<view class="personal_first">
				<view class="personal_first_left">
					性别
				</view>
				<view class="personal_second_right">
					 <view class="second_right_null" v-if="personalDataObj.sex === null" @click="jumpSex">点击此处设置性别</view>
					 <view class="second_right_sex" v-else-if="personalDataObj.sex===0" @click="jumpSex">女</view>
					 <view class="second_right_sex" v-else-if="personalDataObj.sex===1" @click="jumpSex">男</view>
					 <image src="../../static/picture/right.png" mode="" class="first_right_img2"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data(){
		return {
			// personalDataObj: {},
			showHide: false
		}
	},
	onLoad(options) {
		// this.personalDataObj = JSON.parse(options.personalData)
		// console.log(this.personalDataObj);
	},
	methods:{
		//去设置性别
		jumpSex(){
			uni.navigateTo({
				url: `../sexData/sexData?personalObj=${JSON.stringify(this.personalDataObj)}`
			})
		},
		//设置生日
		jumpSetBirthday(){
			uni.navigateTo({
				url: `../birthdayData/birthdayData?personalObj=${JSON.stringify(this.personalDataObj)}`
			})
		},
		//上传头像
		upLoadAva(){
			uni.showActionSheet({
				itemList: ['查看我的头像','从手机相册中选择'],
				success: (res) => {
					// console.log(res);
					if (res.tapIndex === 0){
						this.showHide = true
					}else if(res.tapIndex === 1){
						// let _this = this
					   uni.chooseImage({
					   	  success:(res) => {
					   	  	const tempFilePaths = res.tempFilePaths;
							console.log(tempFilePaths[0]);
							uni.uploadFile({
								url: `${this.$api}/user/uploadFace?qq=434714873&userId=${this.personalDataObj.id}`,
								filePath: tempFilePaths[0],
								name: 'file',
								header: {
									"headerUserId" : this.personalDataObj.id,
									"headerUserToken": this.personalDataObj.userUniqueToken
								},
								success: (res) => {
									this.$store.state.user = JSON.parse(res.data)
									// this.personalDataObj = JSON.parse(res.data)
									console.log(JSON.parse(res.data));
								}
							})
					   	  }
					   })
					}
				}
			})
		},
		//变回小图
		smallImg(){
			this.showHide = false
		}
	},
	computed: {
	   personalDataObj(){
	       return this.$store.state.user
	    }
	},
}
</script>

<style>
.personal_hide{
	width: 100%;
	height: 90vh;
}	
.personal_hide_dv1{
	width: 100%;
	height: 15vh;
	background-color: #000000;
}
.personal_hide_dv2{
	width: 100%;
	height: 15vh;
	background-color: #000000;
}
.personal_hide_img{
	width: 100%;
	height: 60vh;
}
.personal_first{
	width: 100%;
	height: 100rpx;
	display: flex;
	justify-content: space-between;
	padding: 10rpx;
	align-items: center;
	border-bottom: 4rpx solid #D4D4D4;
}	
.personal_first_left{
	font-size: 35rpx;
}
.personal_first_right{
	display: flex;
	align-items: center;
	padding: 10rpx;
}
.first_right_img1{
	width: 60rpx;
	height: 60rpx;
	border-radius: 50%;
}
.first_right_img2{
	width: 40rpx;
	height: 40rpx;
}
.personal_second_right{
	font-size: 35rpx;
	display: flex;
	align-items: center;
	padding: 10rpx;
}
.second_right_sex{
	font-size: 35rpx;
}
.second_right_null{
	font-size: 35rpx;
}
</style>
