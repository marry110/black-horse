<template>
	<view class="goods_list">
		<goods-list 
		:goods="goods"
		@goGoodsIte="goGoodsDetails"
		></goods-list>
		<!-- 下拉加载 -->
		<view class="isOver" v-if="flag">
			我是有底线的~~~~~
			
		</view>
	</view>
</template>
<script>
	import goodsList from '../../compinents/goodslist/goodslist.vue'
	export default {
		data() {
			return {
				pageIndex:1,
				goods:[],	
				flag:false,
			}
		},
		methods: {
			// 获取商品类表的数据
			async getGoodsList(callBack){
				const res=await this.$myRequest({
					url:'/api/getgoods?pageindex='+this.pageIndex
				})
				console.log(res)
				// 下拉加载解决只有一页的信息 上页+本页信息
				// this.goods=res.data.message
				this.goods=[...this.goods,...res.data.message]
				callBack&&callBack()
			},
			// 导航到商品详情页面
			goGoodsDetails(id){
				uni.navigateTo({
					url:'/pages/goods-detail/goods-detail?id'+id
				})
			}
		},
		components:{
			"goods-list":goodsList,
		},
		onLoad() {
			this.getGoodsList()
		},
		// 上拉加载
		onReachBottom() {
			// 到达底部的要求 如果每页数据10天递加，不满足条件到达底部
			if(this.goods.length<this.pageIndex*10) return this.flag=true
			// 测试达到底部
			console.log('触底了')
			// 每次加载page++
			this.pageIndex++
			// 每次上拉加载每页的数据
			this.getGoodsList()
		},
		// 下拉刷新
		onPullDownRefresh() {
			// 到达底部数据
			console.log('下拉刷新')
			// 刷新数据变成第一页
			this.pageIndex=1
			// 使得数据变成空
			this.goods=[]
			// 禁用上拉加载
			this.flag=false
			setTimeout(()=>{
				this.getGoodsList(()=>{
					uni.stopPullDownRefresh()
				})
			},1000)
		},
	}
</script>
<style lang="scss">
	.goods_list{
		background: #eee;
	}
	.isOver{
		width:100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		background: #fff;
		font-size: 28rpx;
		color: #ccc;
		
	}
</style>
