<template>
	<view>
		<view class="pics">
			<scroll-view class="left" scroll>
				<view 
				@click="leftClick(index,item.id)"
				:class="active===index?'active':''"
				
				 v-for="(item,index) in cate" 
				 :key="item.id">
				 {{item.title}}
				 </view>
				
			</scroll-view>
			<scroll-view  class="right" scroll-y>
				<view 
				class="item"
				v-for="(item,index) in secondData" :key="item.id"
				>
					<image 
					:src="item.img_url" 
					mode=""
					@click="previewImg(item.img_url)"
					></image>
					<text>{{item.title}}</text>
				</view>
				<text v-if="secondData.length===0">暂无数据</text>
			</scroll-view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cate:[],
				active:0,
				secondData:[],
			}
		},
		methods: {
			async getPicsCate(){
				const res=await this.$myRequest({
					url:'/api/getimgcategory'
				})
				console.log(res)
				this.cate=res.data.message
				// 每次进入界面左边菜单自动对应二级菜单数据
				this.leftClick(0,this.cate[0].id)
			},
			
			onLoad(){
				this.getPicsCate()
			},
			// 提取二级菜单数据
			async leftClick(index,id){
				// 点击每一个index变色
				console.log(index)
				this.active=index
					console.log(id)
					// 点击一级菜单同时获取二级菜单的id
				const res=await this.$myRequest({
					url:'/api/getimages/'+id,		
				})
				console.log(res)
				this.secondData=res.data.message
			},
			previewImg(current){
				// 循环每一图片
				const urls=this.secondData.map(item=>{
					return item.img_url
				})
				console.log(urls)
				// 获得图片数据AA
				uni.previewImage({
				   current,
				   urls
				})
			}
			
		}
	}
</script>

<style lang="scss">
page{
		height: 100%;
	}
.pics{
	height: 100%;
	display: flex;
	.left{
		width:200rpx;
		height: 100%;
		border-right:1px solid #eee;
		view{
		line-height: 60px;
		height: 60px;
		color:#333;
		text-align: center;
		font-size:30rpx;
		border-top:1px solid #eee;
		}
		.active{
			background: $shop-color;
			color:#fff;
		}
	}
	.right{
		height:100%;
		width:530rpx;
		margin:0 auto;
		.item{
			image{
				width:530rpx;
				height: 530rpx;
				border-radius: 5px;
			}
			text{
				font-size: 30rpx;
				line-height: 60rpx;
			}
		}
	}
}
</style>
