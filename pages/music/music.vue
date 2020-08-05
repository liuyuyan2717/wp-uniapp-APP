<template>
	<view>
		<view class="itembody" v-for="(item,index) in music">
		<u-card :title="item.name" :sub-title="item.author">
			<view class="" slot="body">
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<image :src="item.poster" mode="aspectFill"></image>
					<view class="u-body-item-title u-line-2"></view>
					<button @click="change(index)" type="primary" :disabled="item.act" plain size="mini">点击播放</button>
				</view>
			</view>
		</u-card>
		</view>
	</view>
</template>
<script>
	const innerAudioContext = uni.createInnerAudioContext();
	innerAudioContext.autoplay = true;
	innerAudioContext.onPlay(() => {
			console.log('创建完成');
		});
	
	export default {
		data() {
			return {
				music: [],
				audioAction: {
					method: 'pause'
				},
				aa:1
			}
		},
		onLoad() {
		this.getMusic()
		innerAudioContext.onEnded(()=>{
			for(let item of this.music){
				item.act=false
			}
		})
		},
		methods: {
			//请求服务器音乐列表
			getMusic(){
				uni.request({
					url:'你的域名/music.php',
					sslVerify:false,
					success:res=>{
						this.music=res.data
					}
				})
			},
		
		
			creat() {
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.autoplay = true;
				innerAudioContext.onPlay(() => {
						console.log('创建完成');
					});
			},
			change(index) {
				// console.log(index)
				innerAudioContext.src = this.music[index].src;
				for(let item of this.music){
					item.act=false
				}
				this.music[index].act=true
				
			},
			pause() {}
		}
	}
</script>
<style>
	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		font-size: 32rpx;
		color: #333;
		padding: 1rpx 1rpx;
		background-color: #ececec;

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
</style>
