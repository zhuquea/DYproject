<template>
	<view class="">
		<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
		     <view class="uni-input">{{birDataObj.birthday}}</view>
		</picker>
		<view class="sexData_second">
			<button type="primary" class="sexData_second_btn" @click="submitSex">提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
		const currentDate = this.getDate({
			   format: true
		    })
			return {
                birDataObj: {},
				date: currentDate
			}
		},
		computed: {
		   startDate() {
		          return this.getDate('start');
		    },
		   endDate() {
		        return this.getDate('end');
		    }
		},
		onLoad(options) {
			this.birDataObj = JSON.parse(options.personalObj)
		      console.log(this.birDataObj);
		},
		methods:{
			bindDateChange(e){
				console.log(e.detail.value);
				uni.request({
					url: `${this.$api}/user/modifyUserinfo?qq=434714873`,
					method: 'POST',
					data: {
						userId: this.birDataObj.id,
						birthday: e.detail.value
					},
					header: {
						"headerUserId" : this.birDataObj.id,
						"headerUserToken": this.birDataObj.userUniqueToken
					},
					success: res => {
						this.$store.state.user = res.data.data
						this.birDataObj = res.data.data
						console.log(res.data.data);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getDate(type) {
			            const date = new Date();
			            let year = date.getFullYear();
			            let month = date.getMonth() + 1;
			            let day = date.getDate();
			
			            if (type === 'start') {
			                year = year - 60;
			            } else if (type === 'end') {
			                year = year + 2;
			            }
			            month = month > 9 ? month : '0' + month;;
			            day = day > 9 ? day : '0' + day;
			            return `${year}-${month}-${day}`;
			        },
			submitSex(){
				uni.navigateTo({
					url: `../personalData/personalData?personalData=${JSON.stringify(this.birDataObj)}`
				})
			}		
		}
	}
</script>

<style>
.first_input{
	font-size: 35rpx;
	width: 400rpx;
	line-height: 60rpx;
	padding-left: 40rpx;
}	
.sexData_second{
	margin-top: 20rpx;
	text-align: center;
}	
.sexData_second_btn{
	width: 80%;
	
}
</style>
