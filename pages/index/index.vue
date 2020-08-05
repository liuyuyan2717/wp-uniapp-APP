<template>
	<view>
		<!-- 滚动栏 -->
		<view>
			<u-notice-bar mode="horizontal" :list="rollList"></u-notice-bar>
		</view>
		<!-- 轮播图 -->
		<view class="wrap">
			<u-swiper :list="swiperList" :effect3d="true" duration="500" :circular="true" :title="true" @click="swiperIdex"></u-swiper>
		</view>
		<!-- 搜索文章功能 -->
		<u-search placeholder="搜索一下" input-align="center" @custom="search" @search="search" v-model="keyword"></u-search>
		<!-- 最新文章 -->
		<u-card :title="title" :sub-title="subTitle" :thumb="thumb">
			<view class="" slot="body">
				<view v-for="item in pages" @click="nevigate(item.id)" class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title u-line-2">{{item.title.rendered}}</view>
					<image :src="item.image"  mode="aspectFill"></image>
				</view>
			</view>
			<view class="" slot="foot">
				<u-icon name="chat-fill" size="34" color="" label="131评论---其实并没有人评论"></u-icon>
			</view>
		</u-card>
		<view class="di" v-if="flag"> ------我是有底线的------ </view>
		<view class="aa"><text>{{aa}}</text></view>
		<!-- 分类块 -->

	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList: [],
				rollList: [
					'APP开发全过程请参考我的博客',
					'部分功能暂未开发',
					'如果看的人多，后期会把APP开发完整',
					'干就完了，奥利给！！！'
				],
				title: '最新文章',
				subTitle: '',
				thumb: '你的头像图片地址',
				pageIndex: 1,
				pages: [],
				keyword: '',
				flag:true,
				aa:0

			}
		},
		onLoad() {
			//获取轮播图
		},
		onShow() {
			console.log('页面显示')
			this.getSwiper()
			this.getPagesImage()
		},
		onReachBottom() {
			this.getPagesImage()
		},
		onPullDownRefresh() {
			setTimeout(()=>{
				this.aa+=1
				uni.stopPullDownRefresh()
				},2000)
		},
		methods: {
			getSwiper() {
				uni.request({
					url: '你的域名swiper.php?flag=9',
					sslVerify: false,
					success: res => {
						this.swiperList = res.data
					},
					fail(err) {
						console.log(err)
					}
				})
			},
			//轮播图跳转
			swiperIdex(index) {
				uni.request({
					url: '你的域名swiper.php?flag=' + index,
					sslVerify:false,
					success: res => {
						this.nevigate(res.data)
					}
				})
			},
			//获取最新文章
			getPages() {
				return new Promise((resolve,reject)=>{
					if (this.pages.length == this.pageIndex*10-10){
					uni.request({
					url: '你的域名/wp-json/wp/v2/posts?page=' + this.pageIndex,
					sslVerify:false,
					success: res => {
						this.pageIndex++
						this.pages = [...this.pages,...res.data]
						// this.getPagesImage()
						resolve(res)
						},
					fail:err=> {
						reject(err)
					}
					})
				}
				})
			},
			async getPagesImage(){
				await this.getPages()
				var i=this.pages.length
				var a=0
					for(let item of this.pages){
							uni.request({
								url:'你的域名wp-json/wp/v2/media/'+item.featured_media,
								sslVerify:false,
								success:res=> {
									item.image=res.data.guid.rendered
									a++
									console.log(item.image)
									console.log(a,i)
									this.aa++
									this.aa--
								}
				}) 
				}
			},
			nevigate(id){
				console.log('跳转',id)
				uni.navigateTo({
					url:'../blog/blog?id='+id
				})
			},
			search(){
				uni.navigateTo({
					url:'../search/search?key='+this.keyword
				})
			}

		}
	}
</script>

<style lang="scss">
	.wrap {
		padding: 10rpx 10rpx 30rpx 10rpx;
	}


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
	.di{
		width: 100%;
		height: 50rpx;
		line-height: 50rpx;
		text-align: center;
		font-size: 30rpx;
	}
</style>
