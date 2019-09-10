<template>
	<view class="">
		<view class="search_first">
			<input type="text" value="" placeholder="请输入电影信息" v-model="valueStr" class="first_input" @input="inputObj"/>
			<image src="../../static/picture/search.png" mode="" class="first_img"></image>
			<text class="first_text">搜索</text>
		</view>
		<view class="search_second">
			<view class="search_second_item" v-for="(item,index) in searchDataArr" :key="item.id" @click="jumpDetails(item)">
				<image :src="item.poster" mode="" class="second_item_img"></image>
				<view class="second_item_name">
					{{item.name}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				searchDataArr: [],
				page: 1,//查询的下一个页面的页数，第[page]页
				pageSize: 12,//分页的每一页显示的条数
				valueStr: "",
				startNum: 0
			}
		},
		onLoad() {
			this.getSearchData()
		},
		onNavigationBarSearchInputConfirmed(val){
			uni.request({
				url: `${this.$api}/search/list?qq=434714873&keywords=${val.text}&page=${this.page}&pageSize=${this.pageSize}`,
				method: 'POST',
				data: {},
				success: res => {
					this.searchDataArr = res.data.data.rows
					console.log(this.searchDataArr);
				},
				fail: () => {},
				complete: () => {}
			});
		},
		//无限滚动
		onReachBottom: function () {
		     this.startNum = this.pageSize
			 this.pageSize = 12 + this.searchDataArr.length
			 if(this.startNum !== this.pageSize){
				uni.request({
					url: `${this.$api}/search/list?qq=434714873&keywords=${this.valueStr}&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
									res.data.data.rows.slice(this.startNum,this.pageSize).forEach(item => {
										this.searchDataArr.push(item)
									})
								},
					fail: () => {},
					complete: () => {}
				}); 
			 }else if(this.startNum === this.pageSize) {
				   uni.showToast({
				   	    title: '已经到底啦',
						icon: 'none'
				   })
			 }
		},
		methods:{
			//获取数据
			getSearchData(){
				uni.request({
					url: `${this.$api}/search/list?qq=434714873&keywords=${this.valueStr}&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
						this.searchDataArr = res.data.data.rows
						console.log(this.searchDataArr);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//搜索框的input事件
			inputObj(e){
				console.log(e.detail.value);
				this.valueStr = e.detail.value
		         this.getSearchData()
			},
			//进入详情
			jumpDetails(item){
				uni.navigateTo({
					  url: `../details/details?itemId=${JSON.stringify(item)}`
				})
			}
		}
	}
</script>

<style>
.search_first{
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 20rpx;
	position: relative;
}	
.first_img{
	width: 40rpx;
	height: 40rpx;
     position: absolute;
    top: 32rpx;
    left: 24rpx;
}
.first_input{
	    width: 600rpx;
	    height: 60rpx;
	    background-color: #eeeeee;
	    border-radius: 50rpx;
	    font-size: 30rpx;
	    padding-left: 40rpx;
}
.first_text{
	font-size: 30rpx;
}
.search_second{
	display: flex;
	flex-wrap: wrap;
}
.search_second_item{
	width: 32%;
	text-align: center;
	margin-top: 10rpx;
}
.second_item_img{
	width: 200rpx;
	height: 300rpx;
}
.second_item_name{
	width: 200rpx;
	text-align: center;
	font-size: 26rpx;
}
</style>
