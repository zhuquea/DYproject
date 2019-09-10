<template>
	<view>
		<view class="home_first">
			<swiper class="home_first_swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			    <swiper-item v-for="(item,index) in swiperData" :key="item.id">
			       <image :src="item.image" mode="" class="first_swiper_img"></image>                        
			    </swiper-item>                   
			</swiper>
		</view>
		<view class="home_second_1">
			<image src="../../static/picture/hot.png" mode="" class="second_1_img"></image>
			<text class="second_1_content">热门超英</text>
		</view>
		<view class="home_second">
			<scroll-view class="home_second_scroll" scroll-x="true" scroll-left="120">
			     <view class="second_scroll_dv">
						<view class="second_scroll_item" v-for="(item,index) in hotMovieData" :key="item.id" @click="jumpDetails(item)">
							<image :src="item.cover" mode="" class="scroll_item_img"></image>
							<view class="scroll_item_name">{{item.name}}</view>
							<view class="scroll_item_rate">
								<uni-rate :value="item.Rate" :disabled="disabledBle" size="12"></uni-rate>
								{{item.score}}
							</view>
						</view>
				 </view>
			</scroll-view>
		</view>
		<view class="home_third">
			<image src="../../static/picture/movie-red.png" mode="" class="home_third_img"></image>
			<text class="home_third_content">热门预告</text>
		</view>
		<view class="home_third_video">
			<view class="third_video_item" v-for="(item,index) in hotTrailerData" :key="item.id">
				<video :src="item.trailer" controls :poster="item.poster" class="videoObj" :id="item.id" @play="playObj"></video>
			</view>
		</view>
		<view class="home_fourth">
			<view class="home_fourth_item" v-for="(item,index) in guessLikeData" :key="item.id">
				<view class="fourth_item_1">
					<image :src="item.poster" mode="" class="fourth_item_img"></image>
				</view>
				<view class="fourth_item_2">
					<view class="item_2_name">{{item.name}}</view>
					<view class="item_2_rate">
						<uni-rate :value="item.RateObj" :disabled="disabledBle" size="12"></uni-rate>
					</view>
					<view class="item_2_basicInfo">{{item.basicInfo}}</view>
					<view class="item_2_basicInfo">英雄</view>
					<view class="item_2_basicInfo">{{item.releaseDate}}</view>
				</view>
				<view class="fourth_item_3">
					<image src="../../static/picture/good.png" mode="" class="item_3_img"></image>
					赞一下
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
				swiperData: [],//轮播图数据
				indicatorDots: true,//是否显示面板指示点
				autoplay: true ,//是否自动播放
				interval: 3000, //自动切换时间间隔
				duration: 500 ,//滑动动画时长
				hotMovieData: [],//热门超英数据
				hotTrailerData: [],//热门预告数据
				guessLikeData: [],//‘猜你喜欢’的数据
				disabledBle: true
			}
		},
		onLoad(){
			uni.request({
				url: `${this.$api}/index/carousel/list?qq=434714873`,
				method: 'POST',
				data: {},
				success: res => {
					this.swiperData = res.data.data
					// console.log(this.swiperData);
				},
				fail: () => {},
				complete: () => {}
			});
			this.getHotMovie()
			this.getHotTrailer()
			this.getGuessLike()
		},
		//监听页面隐藏
		onHide: function () {
		
		},
		//监听用户下拉刷新
		onPullDownRefresh: function () {
		   this.getGuessLike()
		   setTimeout(() => {
			   uni.stopPullDownRefresh()
		   },2000)
		},
		//监听用户上拉加载
		onReachBottom: function () {
		
		},
		methods: {
			//获取热门超英数据
			getHotMovie(){
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=434714873&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						this.hotMovieData = res.data.data
						this.hotMovieData.forEach(item => {
							item.Rate = (item.score / 2).toFixed(1)
						})
						// console.log(this.hotMovieData);
					},
					fail: () => {},
					complete: () => {}
				});
			},
		   //获取预告片数据
		   getHotTrailer(){
			   uni.request({
			   	url: `${this.$api}/index/movie/hot?qq=434714873&type=trailer`,
			   	method: 'POST',
			   	data: {},
			   	success: res => {
					this.hotTrailerData = res.data.data
					console.log(this.hotTrailerData);
				},
			   	fail: () => {},
			   	complete: () => {}
			   });
		   },
		   //获取猜你喜欢的数据
		   getGuessLike(){
			   uni.request({
			   	url: `${this.$api}/index/guessULike?qq=434714873`,
			   	method: 'POST',
			   	data: {},
			   	success: res => {
					this.guessLikeData = res.data.data
					this.guessLikeData.forEach(item => {
						item.RateObj = (item.score / 2).toFixed(1)
					})
					console.log(this.guessLikeData);
				},
			   	fail: () => {},
			   	complete: () => {}
			   });
		   },
		   //播放视频事件
		   playObj(e){
			   console.log(e.currentTarget.id);
			   this.hotTrailerData.forEach(item => {
				   if(item.id !== e.currentTarget.id){
				      uni.createVideoContext(item.id).pause()	   
				   }
			   })
			   // let videos = document.getElementsByTagName('video')
			   // // console.log(videos.length);
			   // for(let i = videos.length - 1;i>=0;i--){
				  //  (function(){
				  //                   let p = i;
				  //                   videos[p].addEventListener('play',function(){
				  //                       pauseAll(p);
				  //                   })
				  //               })()
			   // }
			   // function pauseAll(index){
			   //              for (let j = videos.length - 1; j >= 0; j--) {
			   //                  if (j!=index) videos[j].pause();
			   //              }
			   //  
			   //          }
		   },
		   //进入详情页
		   jumpDetails(item){
			   uni.navigateTo({
			   	  url: `../details/details?itemId=${JSON.stringify(item)}`
			   })
		   }
		}
	}
</script>

<style>	
.home_first_swiper{
	width: 100%;
	height: 400rpx;
}
.first_swiper_img{
	width: 100%;
	height: 400rpx;
}	
.home_second_1{
	display: flex;
	align-items: center;
	padding: 20rpx;
}
.second_1_img{
	width: 40rpx;
	height: 40rpx;
}
.second_1_content{
	margin-left: 10rpx;
	font-size: 36rpx;
}
.home_second_scroll{
	width: 100%;
	padding-right: 20rpx;
}
.second_scroll_dv{
	display: flex;
	flex-wrap: nowrap;
}
.second_scroll_item{
	width: 200rpx;
	text-align: center;
	margin-left: 20rpx;
}
.scroll_item_img{
	width: 180rpx;
	height: 180rpx;
}
.scroll_item_name{
	width: 180rpx;
	font-size: 30rpx;
	overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
}
.scroll_item_rate{
	width: 180rpx;
	font-size: 26rpx;
	display: flex;
}
.home_third{
	display: flex;
	align-items: center;
	padding: 20rpx;
}
.home_third_img{
	width: 40rpx;
	height: 40rpx;
}
.home_third_content{
	margin-left: 10rpx;
	font-size: 36rpx;
}
.home_third_video{
	display: flex;
	flex-wrap: wrap;
}
.third_video_item{
	width: 48%;
	height: 350rpx;
	margin-left: 10rpx;
	margin-top: 10rpx;
}
.videoObj{
	width: 100%;
	height: 350rpx;
}
.home_fourth{
	margin-top: 10rpx;
}
.home_fourth_item{
	display: flex;
	justify-content: space-between;
}
.fourth_item_1{
	margin-left: 10rpx;
}
.fourth_item_img{
	width: 150rpx;
	height: 200rpx;
}
.item_2_name{
	font-size: 35rpx;
}
.item_2_basicInfo{
	font-size: 25rpx
}
.item_3_img{
	width: 50rpx;
	height: 50rpx;
}
.fourth_item_3{
    text-align: center;
	display: flex;
	align-items: center;
	width: 150rpx;
	height: 200rpx;
	font-size: 30rpx;
	color: #C4BD36;
	border-left: 2rpx dashed #8F8F94;
	margin-top: 10rpx;
}
</style>
