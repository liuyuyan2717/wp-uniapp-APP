<template>
	<view>
		<!-- 标题 -->
		<view class="title">
			<text>{{title}}</text>
		</view>
		<!-- 头像 -->
		<view class="touxiang">
			<u-avatar :src="src" :show-sex='true'></u-avatar>
			<text>本文与{{date}}创作</text>
		</view>
		<!-- 内容 -->
		<view class="u-content">
			<u-parse :html="content"></u-parse>
		</view>
		<!-- 评论 -->
		<view class="pl">
			<view class="t"><text>这是一个假的评分按钮</text></view>
			<u-rate :count="count" v-model="value" @change="pop"></u-rate>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content: '',
				title: '',
				date: '',
				src: 'https://s2.ax1x.com/2020/02/09/1fx4oD.jpg',
				count: 5,
				value: 0
			}
		},
		onLoad(option) {
			this.getPageInfo(option.id)

		},
		methods: {
			getPageInfo(id) {
				uni.request({
					url: '你的域名.com/wp-json/wp/v2/posts/' + id,
					sslVerify: false,
					success: res => {
						this.title = res.data.title.rendered
						this.content = res.data.content.rendered
						this.date = res.data.date_gmt
					}
				})
			},
			pop(){
				if(this.value==4){
					uni.showToast({
						title:'感谢五星好评！'
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	.title {
		font-size: 50rpx;
		text-align: center;
		margin-top: 30rpx;
		margin-bottom: 10rpx;
	}

	.touxiang {
		text {
			font-size: 30rpx;
		}
	}

	.u-content {
		margin-top: 30rpx;
		margin-left: 60rpx;
		margin-right: 60rpx;
		color: $u-content-color;
		font-size: 32rpx;
		line-height: 1.8;

		// 标签形式无效
		p {
			color: $u-tips-color;
		}
	}
	
	.pl{
		.t{
			margin-top: 30rpx;
			margin-bottom: 30rpx;
			font-size: 30rpx;
		}
		text-align: center;
		margin-bottom: 40rpx;
	}
</style>
