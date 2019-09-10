<template>
	<view class="">
<!-- 		#ifdef H5 -->
		<view class="H5_first">
				<image src="../../static/picture/tou1.jpg" mode="" class="H5_first_img"></image>
		</view>
		<view class="H5_second">
			<input type="text" value="" v-model="h5Name" placeholder="请输入用户名" class="H5_second_name"/>
			<view class="H5_second_1">
				账号
			</view>
		</view>
		<view class="H5_second">
			<input type="password" value="" v-model="h5Password" placeholder="请输入密码" class="H5_second_password"/>
			<view class="H5_second_2">
				密码
			</view>
		</view>
		<view class="H5_third">
			<button type="primary" class="H5_third_btn" @click="H5Login">登录/注册</button>
		</view>
<!-- 		#endif -->
<!--        #ifdef MP-WEIXIN -->
        <view class="WEIXIN_first">
        	<image src="../../static/picture/tou1.jpg" mode="" class="WEIXIN_first_img"></image>
        </view>
		<view class="H5_second">
			<input type="text" value="" v-model="WEIXINName" placeholder="请输入用户名" class="WEIXIN_second_name"/>
			<view class="WEIXIN_second_1">
				账号
			</view>
		</view>
		<view class="H5_second">
			<input type="password" value="" v-model="WEIXINPassword" placeholder="请输入密码" class="WEIXIN_second_password"/>
			<view class="WEIXIN_second_2">
				密码
			</view>
		</view>
		<view class="H5_third">
			<button type="primary" class="H5_third_btn" @click="WEIXINLogin">登录/注册</button>
		</view>
		<view class="WEIXIN_fourth">
			<view class="WEIXIN_fourth_1">
				第三方账号登录
			</view>
			<image src="../../static/picture/weixin.png" mode="" class="WEIXIN_fourth_2" @click="thirdLogin"></image>
		</view>
<!--         #endif -->
<!-- #ifdef APP-PLUS -->
        <view class="WEIXIN_first">
        	<image src="../../static/picture/tou1.jpg" mode="" class="WEIXIN_first_img"></image>
        </view>
		<view class="H5_second">
			<input type="text" value="" v-model="appPLUSName" placeholder="请输入用户名" class="WEIXIN_second_name"/>
			<view class="WEIXIN_second_1">
				账号
			</view>
		</view>
		<view class="H5_second">
			<input type="text" value="" v-model="appPLUSPassword" placeholder="请输入密码" class="WEIXIN_second_password"/>
			<view class="WEIXIN_second_2">
				密码
			</view>
		</view>
		<view class="H5_third">
			<button type="primary" class="H5_third_btn" @click="appPlusLogin">登录/注册</button>
		</view>
		<view class="WEIXIN_fourth">
			<view class="WEIXIN_fourth_1">
				第三方账号登录
			</view>
			<view class="APP-PLUS_fourth_img">
				<image src="../../static/picture/weixin.png" mode="" class="WEIXIN_fourth_2" @click="thirdLogin"></image>
				<image src="../../static/picture/QQ.png" mode="" class="WEIXIN_fourth_2" @click="thirdLogin"></image>
				<image src="../../static/picture/weibo.png" mode="" class="WEIXIN_fourth_2" @click="thirdLogin"></image>
			</view>
		</view>
<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data(){
			return {
				h5Name: "",
				h5Password: "",
				WEIXINName: "",
				WEIXINPassword: "",
				appPLUSName: "",
				appPLUSPassword: "",
				logintype: ''
			}
		},
		onLoad() {
		
		},
		methods:{
			//H5登录注册
			H5Login(){
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=434714873&username=${this.h5Name}&password=${this.h5Password}`,
					method: 'POST',
					data: {
						username: this.h5Name,
						password: this.h5Password
					},
					success: res => {
						console.log(res);
						if(res.data.status === 500){
							uni.showToast({
								title: res.data.msg,
								icon: 'none'
							})
						}else if(res.data.status === 200){
							this.$store.state.user = res.data.data
							// console.log(this.$store.state.user);
							uni.switchTab({
								url: `../myCenter/myCenter`
							})
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//微信登录注册
			WEIXINLogin(){
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=434714873&username=${this.WEIXINName}&password=${this.WEIXINPassword}`,
					method: 'POST',
					data: {
						username: this.WEIXINName,
						password: this.WEIXINPassword
					},
					success: res => {
						console.log(res.data);
						if(res.data.status === 500){
							uni.showToast({
								title: res.data.msg,
								icon: 'none'
							})
						}else if(res.data.status === 200){
							this.$store.state.user = res.data.data
							// console.log(this.$store.state.user);
							uni.switchTab({
								url: `../myCenter/myCenter`
							})
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//app登录注册
			appPlusLogin(){
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=434714873&username=${this.appPLUSName}&password=${this.appPLUSPassword}`,
					method: 'POST',
					data: {},
					success: res => {
						console.log(res);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//微信第三方登录
			thirdLoginWEI(e){
					let user = e.detail.userInfo
						console.log(e);
						console.log(user);
							// 微信登录
							uni.login({
								provider: 'weixin',
								success: res => {
									// 获得微信登录的授权码
									let code = res.code
									uni.request({
										url: `${this.$api}/stu/mpWXLogin/${code}`,
										method: 'POST',
										header: {
											'qq': '122212489'
										},
										data: {
											avatarUrl: user.avatarUrl,
											nickName: user.nickName,
											whichMP: 1
										},
										success: res => {
											console.log(res);
											if (res.data.status === 200) {
				                               
											}
										}
									})
								}
							})
			},
			thirdLogin(e){
				this.logintype = e.currentTarget.dataset.logintype
				console.log(this.logintype);
					uni.login({
						provider: this.logintype,
						success: res => {
						   console.log(res)
								// 成功以后 获取用户的信息
							uni.getUserInfo({
							   provider: this.logintype,
								success: response => {
								// console.log(JSON.stringify(response));
									let face = ''
									let nickname = ''
								   let openIdOrUid = ''
									let userInfo = response.userInfo
										if (this.logintype === 'weixin') {
										 face = userInfo.avatarUrl
										 nickname = userInfo.nickName
										   openIdOrUid = userInfo.openId
										} else if (this.logintype === 'qq') {
										face = userInfo.figureurl_qq_2
										nickname = userInfo.nickname
										openIdOrUid = userInfo.openId
									} else if (this.logintype === 'sinaweibo') {
										face = userInfo.avatar_large
										nickname = userInfo.nickname
										openIdOrUid = userInfo.id
								}
											// 使用一键登录
									uni.request({
										url: `${this.$api}/appUnionLogin/${this.logintype}?qq=122212489`,
										method: 'POST',
										data: {
											face: face,
											openIdOrUid: openIdOrUid,
											nickname: nickname
										},
										success: res => {
											if (res.data.status === 200) {
												console.log(res.data.data);
												let userInfo = res.data.data
												uni.setStorageSync('user', userInfo)
												uni.switchTab({
													url: '/pages/me/me'
												})
											}
				
										},
											fail: () => {},
											complete: () => {}
										});
									}
								})
								},
					    fail: () => {},
					  complete: () => {}
				});
			}
		},
		computed: {
	
	    }
	}
</script>

<style>
.H5_first{
	text-align: center;
}
.H5_first_img{
	width: 150rpx;
	height: 150rpx;
	border-radius: 50%;
	margin-top: 35%;
}	
.H5_second{
	text-align: center;
	line-height: 30rpx;
	position: relative;
	margin-top: 20rpx;
}
.H5_second_name{
	width: 600rpx;
	height: 60rpx;
	font-size: 30rpx;
	padding-left: 40rpx;
	box-shadow: 2rpx 2rpx 8rpx 6rpx rgba(0,0,0,0.1);
	margin: 0 auto;
}
.H5_second_password{
	width: 600rpx;
	height: 60rpx;
	font-size: 30rpx;
	padding-left: 40rpx;
	box-shadow: 2rpx 2rpx 8rpx 6rpx rgba(0,0,0,0.1);
	margin: 0 auto;
}
.H5_second_1{
	font-size: 35rpx;
	position: absolute;
	top: 15rpx;
	left: 100rpx;
}
.H5_second_2{
	font-size: 35rpx;
	position: absolute;
	top: 15rpx;
	left: 100rpx;
}
.H5_third_btn{
	width: 80%;
	margin: 60rpx auto;
}
.WEIXIN_first{
	text-align: center;
}
.WEIXIN_first_img{
	width: 150rpx;
	height: 150rpx;
	border-radius: 50%;
	margin-top: 25%;
}
.WEIXIN_second_name{
	width: 600rpx;
	height: 80rpx;
	font-size: 36rpx;
	padding-left: 40rpx;
	box-shadow: 2rpx 2rpx 8rpx 6rpx rgba(0,0,0,0.1);
	margin: 0 auto;
}
.WEIXIN_second_password{
	width: 600rpx;
	height: 80rpx;
	font-size: 36rpx;
	padding-left: 40rpx;
	box-shadow: 2rpx 2rpx 8rpx 6rpx rgba(0,0,0,0.1);
	margin: 0 auto;
}
.WEIXIN_second_1{
	font-size: 38rpx;
	position: absolute;
	top: 24rpx;
	left: 100rpx;
}
.WEIXIN_second_2{
	font-size: 38rpx;
	position: absolute;
	top: 24rpx;
	left: 100rpx;
}
.WEIXIN_fourth{
	text-align: center;
	line-height: 50rpx;
}
.WEIXIN_fourth_1{
	font-size: 28rpx;
	color: #808080;
}
.WEIXIN_fourth_2{
	width: 80rpx;
	height: 80rpx;
	border-radius: 50%;
}
.APP-PLUS_fourth_img{
	display: flex;
	justify-content: space-between;
}
</style>
