<template>
	<view class="container">
		<view class="intro">本项目已包含uni ui组件，无需import和注册，可直接使用。在代码区键入字母u，即可通过代码助手列出所有可用组件。光标置于组件名称处按F1，即可查看组件文档。</view>
		<text class="intro">详见：</text>
		<uni-link :href="href" :text="href"></uni-link><br>
		<uni-link :href="url" :text="url"></uni-link>
		<button type="default" @tap="navigateTo">跳转到新页面</button>
		<button type="default" @tap="redirectTo">在当前页打开</button>
		<button type="default" @tap="switchTab">跳转到tab页面</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				href: 'https://uniapp.dcloud.io/component/README?id=uniui',
				url: "/pages/login/login",
			}
		},
		methods: {
			navigateTo() {
				uni.navigateTo({
					url: this.url
				})
			},
			redirectTo() {
				uni.redirectTo({
					url: this.url
				});
			},
			switchTab() {
				uni.switchTab({
					url: this.url
				});
			}
		}
	}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
