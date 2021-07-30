<template>
	<view class="home">
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular="true">
			<swiper-item v-for="(item,index) in swiper" :key="item.id">
				<view class="swiper-item">
					<image :src="item.img" id="iconimg">
						
					</image>
				
				</view>
			</swiper-item>
		
		</swiper>
		
		<!-- 导航区域 -->
		<view class="nav">
			<view class="nav_item" v-for="(item,index) in navs" :key="item.id" @click="navItemClick(item.path)">
				<view :class="item.icon">
					
				</view>
				<text>{{item.title}}</text>
			</view>
			<!-- <view class="nav_item">
				<view class="iconfont icon-guanyuwomen">
					
				</view>
				<text>联系我们</text>
			</view>
			<view class="nav_item">
				<view class="iconfont icon-tupian">
					
				</view>
				<text>社区图片</text>
			</view>
			<view class="nav_item">
				<view class="iconfont icon-shipin">
					
				</view>
				<text>学习视频</text>
			</view> -->
		</view>
		 <!-- 推荐商品列表 -->
		<view class="hot_goods">
			<view class="tit">
				推荐商品
				
			</view>
			
			<goods-list 
			:goods="goods"
			@goGoodsItem="goGoodsDetails"
			></goods-list>
		</view>
	</view>  
</template>

<script>
	import goodsList from '../../compinents/goodslist/goodslist.vue'
	export default {
		data() {
			return {
				swiper: [],
				goods:[],
				navs:[
					{
						icon:'iconfont icon-ziyuan',
						title:'翻斗超市',
						path:'/pages/goods/goods'
						
					},
					{
						icon:'iconfont icon-guanyuwomen',
						title:'联系我们',
						path:'/pages/contact/contact'
						
					},
					{
						icon:'iconfont icon-tupian',
						title:'社区图片',
						path:'/pages/pics/pics',
						
						
					},
					{
						icon:'iconfont icon-shipin',
						title:'学习视频',
						path:'/pages/videos/videos'
						
					}
				]
			}
		},
		onLoad() {
			this.geSwiperdata()
			this.getHotGoods()
		},
		components:{
			"goods-list":goodsList,
		},
		methods: {
			// 获取轮播图数据
			// geSwiperdata() {
			// 	console.log('获取轮播图数据'),
			// 		uni.request({
			// 			url: '/api/getlunbo',
			// 			// url:'https://api-hmugo-web.itheima.net/api/public/v1/home/swiperdata',
			// 			success: res => {
			// 				console.log(res)
			// 				if (res.data.status !== 0) {
			// 					return uni.showToast({
			// 						title: '获取数据失败',
			// 					})
			// 				}
			// 				this.swiper = res.data.message
			// 			}
			// 		})
				
				
			// }
			
			// async geSwiperdata(){
			// 	const res=await uni.request({
			// 		url: 'http://localhost:8082/api/getlunbo',
					
			// 	})
			// 	console.log(res)
			// }
			
			async geSwiperdata(){
				const res=await this.$myRequest({
					url:'/api/getlunbo',
				})
				console.log(res)
				this.swiper=res.data.message  
			},
			// 获取热门商品列表
			async getHotGoods(){
				const res=await this.$myRequest({
					url:'/api/getgoods?pageindex=1'
				})
				console.log(res)
				this.goods=res.data.message
				
			},
			// 导航点击
			navItemClick(url){
				console.log(url)
				uni.navigateTo({
					url
				})
			
			
		},
		// 导航到商品详情页面
		goGoodsDetails(id){
			uni.navigateTo({
				url:'/pages/goods-detail/goods-detail?id'+id
			})
		},
		
	},
	}
</script>

<style lang="scss">
	.home{
		swiper{
			max-width: 750rpx;
			max-height: 380rpx;
			image{
				max-height: 100%;
				width: 100%;
			}
		}
	}

.nav{
	display: flex;
	.nav_item{
		width: 25%;
		flex:1;
		text-align: center;
		justify-content: center;
		view{
			width:120rpx;
			height: 120rpx;
			border-radius: 50%;
			background-color:$shop-color;
			margin: 10px auto;
			line-height: 120rpx;
			color:#fff;
			font-size: 50rpx;
		}
		.iconfont{
			font-size: 45rpx;
		}
		text{
			font-size: 30rpx;
		}
	}
	
	}
	
	.hot_goods{
		background-color: #eee;
		overflow: hidden;
		margin-top: 10rpx;
		.tit{
			height: 50px;
			line-height: 50px;
			color:$shop-color ;
			text-align: center;
			letter-spacing: 20px;
			background: #fff;
			margin: 5rpx 0;
		}
		}
</style>


