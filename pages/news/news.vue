<template>
	<view class="news">
		<view class="">
			<new-item 
			:list="newsList"
			@itemClick="goDetail"
			></new-item>
		</view>
	</view>
</template>

<script>
	import newsItem from '../../compinents/new-item/newitem.vue'
	export default {
		data() {
			return {
				newsList:[]
			}
		},
		onLoad(){
			this.getNews()
		},
		methods: {
			async getNews(){
				const res=await this.$myRequest({
					url:'/api/getnewslist',
				})
				console.log(res)
				this.newsList=res.data.message
			},
			goDetail(id){
				console.log(id)
				uni.navigateTo({
					url:'/pages/news-details/news-details?id'+id
				})
			}
		
		},
		components:{
			"new-item":newsItem
		}
	}
</script>

<style lang="scss">
	.news{
		.news_item{
			display: flex;
			padding: 10rpx 20rpx;
			border-bottom:1px solid $shop-color;
			image{
				width:200rpx;
				min-width: 200rpx;
				max-width: 200rpx;
				height: 150rpx;
				
			}
			.right{
				margin-left:15rpx;
				display: flex;
				flex-direction:column;
				justify-content: space-between;
				.tit{
					font-size:30rpx;
				}
				.info{
					font-size:24rpx;
					text:nth-child(2){
						margin-left:30rpx;
					}
				}
			}
			
		}
	}

</style>
