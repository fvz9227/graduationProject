/**
* author: Zhangys
* description: 检验报告列表
* @createTime: 2022-05-05 13:18:17
*/
<template>
	<view class="container">
		<view class="item g-flex g-flex-column g-flex-center" v-for="(item,index) in reportList" :key="index"
			@click="gotoDetail(item)">
			<view class="item_content">
				<view class="item_name">
					{{item.hsName}}
				</view>
				<view class="item_date">
					预约时间：{{item.hsDate}}
				</view>
				<view class="item_date" v-if="item.hsTrueDate">
					检测时间：{{item.hsTrueDate}}
				</view>
				<view class="item_img">
					<image v-if="item.status=='1'" class="img_class1" src="../../static/yifabu.png" mode=""></image>
					<image v-else class="img_class2" src="../../static/weifabu.png" mode=""></image>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	import {
		getOpenId
	} from '@/utils/storage.js';
	export default {
		name: '',
		components: {

		},
		data() {
			return {
				reportList: [],
				formData: {
					openid: "",
				}
			}
		},
		computed: {

		},
		created() {
			this.formData.openid = getOpenId().openid
			this.getReport()
		},
		methods: {
			getReport() {
				this.$cloud({
					name: "hsAppoinment",
					data: {
						...this.formData
					}
				}).then(res => {
					console.log(res);
					if (res.code == '0') {
						this.reportList = res.data
					} else {

					}
				})
			},
			gotoDetail(item) {
				if (item.status == '0') {
					uni.showModal({
						title: '提示',
						content: '检测后将在4-6小时后出结果，请稍后再试',
					})
					return
				}
				let params = JSON.stringify(item)
				uni.navigateTo({
					url: './detail?params=' + params
				})
			},
		},
	}
</script>

<style lang='scss'>
	page {
		height: auto;
		/* background-color: #FFFFFF; */
	}

	.container {
		margin: 30rpx 30rpx;

		.item {
			position: relative;
			height: 180rpx;
			width: 100%;
			margin-bottom: 40rpx;
			border-radius: 15rpx;
			background-color: #f8f8f8;
			box-shadow: 0 1rpx 16rpx rgba(0, 0, 0, 0.1);

			.item_content {
				margin: 0 20rpx;

				.item_name {
					font-size: 44rpx;
					/* font-weight: bold; */
					margin-bottom: 20rpx;
				}

				.item_date {
					font-size: 30rpx;
				}

				.item_img {
					position: absolute;
					right: 44rpx;
					top: 58rpx;

					.img_class1 {
						width: 120rpx;
						height: 120rpx;
					}

					.img_class2 {
						width: 130rpx;
						height: 100rpx;
					}
				}
			}

		}
	}
</style>
