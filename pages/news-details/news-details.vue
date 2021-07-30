<template>
	<view>
		<view class="news_detail">
			<text class="title">{{detail.title}}</text>
			<view class="info">
				<text>{{detail.add_time|formatDate}}</text>
				<text>浏览:{{detail.click}}</text>
				
			</view>
			<view class="content">
				<!-- {{detail.content}} -->
				<rich-text :nodes="detail.content"></rich-text>
			</view>
			
		</view> 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:0,
				details:[],
				
			}
		},
		methods: {
			async getNewsDetail(){
				const res=await this.$myRequest({
					url:'/api/getnew/'+this.id
				})
				console.log(res)
				this.detail=res.data.message[0]
			}
		},
		onLoad(options){
			console.log(options)
			this.id=options.id
			this.getNewsDetail()
			
		}
	}
</script>

<style lang="scss">
	.news_detail{
		font-size:30rpx;
		padding: 0 20rpx;
		.title{
			text-align: center;
			width:710rpx;
			display: block;
			margin:20rpx 0;
		}
		.info{
			display: flex;
			justify-content: space-between;
		}
	}

</style>
