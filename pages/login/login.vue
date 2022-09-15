<template>
	<view class="container">
		<uni-section title="Login" type="line">
			<view class="example">
				<uni-forms ref="customForm" :rules="customRules" :modelValue="customFormData">
					<uni-forms-item label="邮箱号" required name="email">
						<uni-easyinput v-model="customFormData.email" placeholder="请输入邮箱号" />
					</uni-forms-item>
					<uni-forms-item label="密码" required name="password">
						<uni-easyinput v-model="customFormData.password" placeholder="请输入密码" />
					</uni-forms-item>
				</uni-forms>
				<view class="button-group">
					<button type="primary" size="mini" @click="login('customForm')">登录</button>
					<button type="primary" size="mini" @click="register()">注册</button>
				</view>
			</view>
		</uni-section>
	</view>
</template>


<script>
	import helper from '../../common/helper.js';

	export default {
		data() {
			return {
				// 自定义表单数据
				customFormData: {
					email: '',
					password: ''
				},
				// 自定义表单校验规则
				customRules: {
					email: {
						rules: [{
							required: true,
							errorMessage: '姓名不能为空'
						}]
					},
					password: {
						rules: [{
							required: true,
							errorMessage: '密码不能为空'
						}]
					}
				},
				registerUrl: "/pages/register/register",
				listItemUrl: "/pages/listitem/listitem"
			}
		},
		onLoad() {},
		onReady() {
			// 设置自定义表单校验规则，必须在节点渲染完毕后执行
			this.$refs.customForm.setRules(this.customRules)
		},
		methods: {
			login(ref) {
				let returnUrl = this.listItemUrl
				this.$refs[ref].validate().then(res => {
					uni.request({
						url: 'http://localhost:8090/user/login',
						header: {
							'Content-Type': 'application/x-www-form-urlencoded'
						},
						method: 'POST',
						data: {
							email: res.email,
							password: res.password
						},
						success(res) {
							console.log('res', res)
							if (res.data.status == "success") {
								uni.showToast({
									title: `登录成功`
								})
								helper.token = res.data.data.token
								uni.navigateTo({
									url: returnUrl
								})
							} else {
								uni.showToast({
									title: `账号和密码错误,登录失败`
								})
							}
						}
					})
				}).catch(err => {
					console.log('err', err);
				})
			},
			register() {
				console.log('跳转注册界面')
				uni.showToast({
					title: `跳转注册界面`
				})
				uni.navigateTo({
					url: this.registerUrl
				})
			},
		}
	}
</script>


<style lang="scss">
	.example {
		padding: 15px;
		background-color: #fff;
	}

	.segmented-control {
		margin-bottom: 15px;
	}

	.button-group {
		margin-top: 15px;
		display: flex;
		justify-content: space-around;
	}

	.form-item {
		display: flex;
		align-items: center;
	}

	.button {
		display: flex;
		align-items: center;
		height: 35px;
		margin-left: 10px;
	}
</style>
