<template>
	<view class="login">
		<view class="content-wrapper">
      <view style="height:330rpx"></view>
			<view class="login-form">
				<view class="login-form-items">
					<view class="login-form-items-title">帐号</view>
					<input type="text" class="login-input" placeholder="帐号" />
				</view>

				<view class="login-form-items">
					<view class="login-form-items-title">登录密码</view>
					<input type="password" class="login-input" placeholder="请输入登录密码" />
				</view>

			</view>
		</view>
		<div class="submit-wrapper">
			<button type="primary" class="login-btn">登录</button>
		</div>
	</view>
</template>

<script lang="ts" setup>
</script>

<style lang="scss" scoped>
	page {
		background: #F4F5F6;
	}

	.login {
		.content-wrapper {
			width: 100%;

			.title {
				margin-top: 35rpx;
				width: 100%;
				margin-bottom: 10px;

				h1 {
					border: 0px;
					width: 50%;
					margin: 0 auto;
					text-align: center;
					border-bottom: 1px solid #E3E3E3;
					height: 50px;
					line-height: 50px;
					font-size: 17px;
					overflow: hidden;
					font-weight: 400;
				}
			}

			.login-form {
				margin: 20px 10px 20px 15px;
				background: #FFFFFF;

				.login-form-items {
					padding: 15px 10px;
					border-bottom: 1px solid #F3F4F5;
					position: relative;
					display: -webkit-flex;
					display: flex;

					.login-form-items-title {
						width: 30%;
						line-height: 22px;
						height: 22px;
						flex-shrink: 0;
					}

					.login-input {
						width: 100%
					}

					img {
						width: auto;
						height: auto;
						max-width: 100%;
						max-height: 100%;
					}
				}
			}
		}

		.submit-wrapper {
			padding: 10px;
			padding-top: 10px;
		}

	}
</style>

