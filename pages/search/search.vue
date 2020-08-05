<template>
	<view>
		<u-card :title="title" :sub-title="subTitle" :thumb="thumb">
			<view class="" slot="body">
				<view v-for="item in pages" @click="nevigate(item.id)" class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title u-line-2">{{item.title.rendered}}</view>
					<image :src="item.image"  mode="aspectFill"></image>
				</view>
			</view>
		</u-card>
		<view class="aa">{{aa}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				title: '搜索结果',
				subTitle: '',
				thumb: '图片.jpg',
				pageIndex: 1,
				pages: [],
				aa:0
			}
		},
		onLoad(option) {
			this.getPagesImage(option.key)
		},
		methods: {
			getPages(key) {
				return new Promise((resolve,reject)=>{
					uni.request({
					url: ' 你的域名/wp-json/wp/v2/posts?search=' + key,
					sslVerify:false,
					success: res => {
						this.pages = res.data
						resolve(res)
						},
					fail:err=> {
						reject(err)
					}
					})
				})
			},
			async getPagesImage(key){
				await this.getPages(key)
					for(let item of this.pages){
							uni.request({
								url:'你的域名/wp-json/wp/v2/media/'+item.featured_media,
								sslVerify:false,
								success:res=> {
									item.image=res.data.guid.rendered
									console.log(item.image)
									this.aa++
									this.aa--
								}
				}) 
				}
			},
			nevigate(id){
				uni.navigateTo({
					url:'../blog/blog?id='+id
				})
			}
			
			
		}
	}
</script>

<style lang="scss">

	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		font-size: 32rpx;
		color: #333;
		padding: 10rpx 10rpx;

		.u-body-item-title {
			width: 600rpx;
		}
	}

	.u-body-item image {
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}
	
	.aa{
		width: 1rpx;
		height: 1rpx;
		text-align: 1rpx;
	}
</style>
