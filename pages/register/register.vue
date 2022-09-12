<template>
	<view class="container">
		<uni-section title="Register" type="line">
			<view class="example">
				<uni-forms ref="customForm" :rules="customRules" :modelValue="customFormData">
					<uni-forms-item label="用户名" required name="name">
						<uni-easyinput v-model="customFormData.name" placeholder="请输入用户名" />
					</uni-forms-item>
					<uni-forms-item label="性别" required name="gender">
						<uni-data-checkbox v-model="customFormData.gender" :localdata="sexs" />
					</uni-forms-item>
					<uni-forms-item label="邮箱号" required name="email">
						<uni-easyinput v-model="customFormData.email" placeholder="请输入邮箱号" />
					</uni-forms-item>
					<uni-forms-item label="密码" required name="password">
						<uni-easyinput v-model="customFormData.password" placeholder="请输入密码" />
					</uni-forms-item>
					<uni-forms-item label="手机号" required name="telphone">
						<uni-easyinput v-model="customFormData.telphone" placeholder="请输入手机号" />
					</uni-forms-item>
					<uni-forms-item label="验证码" required name="otpCode">
						<uni-easyinput v-model="customFormData.otpCode" placeholder="请输入验证码" />
					</uni-forms-item>
					<uni-forms-item label="年龄" required name="age">
						<uni-easyinput v-model="customFormData.age" placeholder="请输入年龄" />
					</uni-forms-item>
				</uni-forms>
				<view class="button-group">
					<button type="primary" size="mini" @click="register('customForm')">注册</button>
					<button type="primary" size="mini" @click="getotp()">获取验证码</button>
					<button type="primary" size="mini" @click="relogin()">返回登录界面</button>
				</view>
			</view>
		</uni-section>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				// 自定义表单数据
				customFormData: {
					age: 0,
					gender: 0,
					email: '',
					name: '',
					otpCode: '',
					password: '',
					registerMode: '',
					telphone: '',
					thirdPartyId: ''
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
					},
					name: {
						rules: [{
							required: true,
							errorMessage: '用户不能为空'
						}]
					},
					otpCode: {
						rules: [{
							required: true,
							errorMessage: '验证码不能为空'
						}]
					},
					telphone: {
						rules: [{
							required: true,
							errorMessage: '电话不能为空'
						}]
					}
				},
				url: "/pages/login/login",
				// 单选数据源
				sexs: [{
					text: '男',
					value: 1
				}, {
					text: '女',
					value: 2
				}, {
					text: '保密',
					value: 0
				}],
			}
		},
		onLoad() {},
		onReady() {
			// 设置自定义表单校验规则，必须在节点渲染完毕后执行
			this.$refs.customForm.setRules(this.customRules)
		},
		methods: {
			register(ref) {
				let returnUrl =  this.url
				this.$refs[ref].validate().then(res => {
					uni.request({
						url: 'http://localhost:8090/user/register',
						header: {
							'Content-Type': 'application/json;charset=UTF-8',
						},
						method: "POST",
						data: {
							age: res.age,
							gender: res.gender,
							email: res.email,
							name: res.name,
							otpCode: res.otpCode,
							password: res.password,
							registerMode: res.registerMode,
							telphone: res.telphone,
							thirdPartyId: res.thirdPartyId
						},
						success(res) {
							console.log('res',res)
							if(res.data.status == "success"){
								uni.showToast({
									title: `注册成功`
								})
								uni.navigateTo({
									url: returnUrl
								})
							}else{
								uni.showToast({
									title: `注册失败`
								})
							}
						}
					})
				}).catch(err => {
					console.log('err', err);
				})
			},
			relogin() {
				uni.showToast({
					title: `返回登录界面`
				})
				uni.navigateTo({
					url: this.url
				})
			},
			getotp() {
				if (this.customFormData.email == '') {
					uni.showToast({
						title: `邮箱为空验证码发送失败`
					})
				} else {
					uni.request({
						url: 'http://localhost:8090/user/get-otp',
						method: "GET",
						header: {
							'Content-Type': 'application/x-www-form-urlencoded'
						},
						data: {
							email: this.customFormData.email
						},
						success(res) {
							console.log("res:=>" + res)
						}
					})
				}
			}
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
