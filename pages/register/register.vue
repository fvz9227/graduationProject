/**
* author: Zhangys
* description: 注册界面
* @createTime: 2022-05-05 13:18:02
*/
<template>
	<view class="login-container">
		<view class="cu-form-group margin-top">
			<text class="cuIcon-people icon_style"></text>
			<input placeholder="请输入用户名" v-model="formData.username" />
		</view>

		<view class="cu-form-group margin-top">
			<text class="cuIcon-lock icon_style"></text>
			<input placeholder="请输入密码" password v-model="formData.password" />
		</view>
		<view class="cu-form-group margin-top">
			<text class="cuIcon-lock icon_style"></text>
			<input placeholder="请再次输入密码" password v-model="formData.password1" @blur="onInputBlur" />
		</view>
		<view class="loing_btn g-flex g-flex-center">
			<button class="cu-btn" @click="registerHandle">
				<text>
					注册用户
				</text>
			</button>
		</view>

		<view class=" goRegister g-flex g-flex-end">
			<view class="">
				已有账号,去
				<text style="color:#4e87ff" @click="loginHandle">登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: '',
		components: {},
		data() {
			return {
				formData: {
					password: '',
					password1: '',
					username: ''
				}
			};
		},
		computed: {},
		created() {

		},
		methods: {
			//表单验证
			validate() {
				if (!this.formData.username) {
					uni.showModal({
						title: "提示",
						content: '请输入用户名',
						showCancel: false,
					});
					return
				}
				if (!this.formData.password) {
					uni.showModal({
						title: "提示",
						content: '请输入密码',
						showCancel: false,
					});
					return
				}
				if (!this.formData.password1) {
					uni.showModal({
						title: "提示",
						content: '请再次输入密码',
						showCancel: false,
					});
					return
				}
				if (this.formData.password != this.formData.password1) {
					uni.showModal({
						title: "提示",
						content: '密码不一致,请重新输入',
						showCancel: false,
					});
					return
				}
				return true
			},
			
			//表单失焦验证两次输入密码是否一致
			onInputBlur() {
				if (this.formData.password != this.formData.password1) {
					uni.showToast({
						title: "两次输入密码不一致,请确认密码！",
						icon: "none"
					});
				}
			},
			//注册
			registerHandle() {
				if (!this.validate()) return
				let params = JSON.parse(JSON.stringify(this.formData))
				delete params.password1
				this.$cloud({
					name: "register",
					data: {
						...params
					}
				}).then(res => {
					console.log(res);
					if (res.code === 0) {
						uni.showToast({
							title: "注册成功！"
						});
						setTimeout(() => {
							uni.navigateTo({
								url: '../login/login?loginForm=' + JSON.stringify(this.formData)
							})
						}, 500)
					} else {
						uni.showToast({
							title: res.msg,
							icon: "error"
						});
					}
				});
			},
			loginHandle() {
				uni.navigateTo({
					url: '../login/login'
				})
			}

		}

	};
</script>

<style lang="scss" scoped>
	.login-container {
		margin: 70rpx 50rpx;

		.cu-form-group {
			border-radius: 10rpx;

			.icon_style {
				margin-right: 10rpx;
			}
		}

		.loing_btn {
			margin-top: 40rpx;

			.cu-btn {
				border-radius: 25px;
				width: 600rpx;
				height: 100rpx;
				color: #fff;
				background-color: #59b58d;
			}
		}

		.goRegister {
			margin-top: 40rpx;
		}
	}
</style>
