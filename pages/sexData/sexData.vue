<template>
	<view class="">
		<view class="sexData_first">
		 <radio-group @change="radioChange">
			<label class="radio"><radio :value="sexData0" />男</label>
			<label class="radio"><radio :value="sexData00" />女</label>
		</radio-group>
		</view>
		<view class="sexData_second">
			<button type="primary" class="sexData_second_btn" @click="submitSex">提交</button>
		</view>
	</view>
</template>

<script>
	// &birthday=${this.sexDataBirthday}&nickname=${this.sexDataNickname}&sex=${this.sexDataNum}&userId=${this.sexDataUserId}
	export default {
		data(){
			return {
				sexData0: '男',
				sexData00: '女',
				// sexData1: 1,
				// sexData2: 0,
				sexDataNum: -1,
				sexDataBirthday: "",
				sexDataNickname: "",
				sexDataUserId: '',
				userUniqueToken: '',
				sexDataObj: {}
			}
		},
		onLoad(options) {
			  this.sexDataObj = JSON.parse(options.personalObj)
			  this.sexDataBirthday = JSON.parse(options.personalObj).birthday
			  this.sexDataNickname = JSON.parse(options.personalObj).username
			  this.sexDataUserId = JSON.parse(options.personalObj).id
			  this.userUniqueToken = JSON.parse(options.personalObj).userUniqueToken
			  // console.log(this.sexDataBirthday);
			  // console.log(this.sexDataNickname);
			  // console.log(this.sexDataUserId);
			  // console.log(this.userUniqueToken);
		},
		methods:{
			radioChange(e){
				console.log(e.detail.value);
				if (e.detail.value === '男') {
					this.sexDataNum = 1
				}else if(e.detail.value === '女'){
					this.sexDataNum = 0
				}
				console.log(this.sexDataNum);
				uni.request({
					url: `${this.$api}/user/modifyUserinfo?qq=434714873`,
					method: 'POST',
					data: {
						userId: this.sexDataUserId,
						birthday: this.sexDataBirthday,
						sex: this.sexDataNum
					},
					header: {
						"headerUserId" : this.sexDataUserId,
						"headerUserToken": this.userUniqueToken
					},
					success: res => {
						this.$store.state.user = res.data.data
						this.sexDataObj = res.data.data
						console.log(this.$store.state.user);
						uni.showToast({
							title: '修改成功'
						})
					},
					fail: () => {},
					complete: () => {}
				});
			},
			submitSex(){
				 uni.navigateTo({
				 	url: `../personalData/personalData?personalData=${JSON.stringify(this.sexDataObj)}`
				 })
			}
		}
	}
</script>

<style>
.sexData_first{
	padding: 10rpx;
}	
.sexData_second{
	margin-top: 20rpx;
	text-align: center;
}	
.sexData_second_btn{
	width: 80%;
	
}
</style>
