<template>
	<view class="My_center_body">
		<view class="my_center_none">
			<view class="center_none_all">
				<image src="../../static/picture/touxiang.jpeg" mode="" class="none_tou_xiang" v-if="userData === null || userData === undefined"></image>
				<image :src="userData.faceImage" mode="" class="none_tou_xiang" v-else></image>
				<view class="none_content" @click="jumpLogin" v-if="userData === null || userData === undefined">
					登录/注册
				</view>
				<view class="my_center_nickname" v-else>
					<view class="">昵称：{{userData.username}}</view>
					<view class="">ID：{{userData.id}}</view>
				</view>
			</view>
		</view>
		<view class="my_center_existence" v-if="userData !== null && userData !== undefined">
		   <view class="my_center_all">
		   	  <view class="my_center_nickname">
		   	  	昵称：{{userData.username}}
		   	  </view>
		   	  <view class="my_center_modify">
		   	  	<button type="primary" class="center_modify" @click="modifyObj">修改个人资料</button>
		   	  </view>
		   </view>
		</view>
		<view class="my_center_out" v-show="userData !== null && userData !== undefined">
			<button type="warn" class="center_out_btn" @click="loginOut">退出登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				
			}
		},
		onLoad() {
			console.log(this.$store.state.user);
			console.log(this.userData);
		},
		methods:{
			jumpLogin(){
				uni.navigateTo({
					url: `../login/login`
				})
			},
			//修改个人资料
			modifyObj(){
				uni.navigateTo({
					url: `../personalData/personalData?personalData=${JSON.stringify(this.userData)}`
				})
			},
			//退出登录
			loginOut(){
				uni.request({
					url: `${this.$api}/user/logout?qq=434714873&userId=${this.userData.id}`,
					method: 'POST',
					data: {},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						console.log(res);
						this.$store.state.user = null
						uni.showToast({
							title: '退出登录成功',
							icon: 'none'
						})
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		computed: {
		   userData(){
		       return this.$store.state.user
		    }
	    },
	}
</script>

<style>
.center_none_all{
	width: 100%;
	height: 200rpx;
	background-color: #C4BD36;
}	
.center_none_all{
	display: flex;
	padding: 20rpx;
	align-items: center;
}
.none_tou_xiang{
	width: 100rpx;
	height: 100rpx;
	border-radius: 50%;
}
.none_content{
	font-size: 40rpx;
	font-weight: bold;
	margin-left: 10rpx;
}
.my_center_all{
	display: flex;
	justify-content: space-between;
	padding: 10rpx;
}
.my_center_nickname{
	font-size: 40rpx;
}
.my_center_modify{
	font-size: 40rpx;
}
.center_modify{
	width: 350rpx;
	line-height: 80rpx;
}
.center_out_btn{
	width: 80%;
	margin: 60rpx auto;
}
.My_center_body{
	width: 100%;
	height: 100vh;
	position: relative;
}
.my_center_out{
	width: 100%;
	position: absolute;
	bottom: 50rpx;
}
</style>
