<template>
	<view class="container">
		<uni-section title="创建商品" type="line">
			<view class="example">
				<uni-forms ref="customForm" :rules="customRules" :modelValue="customFormData">
					<uni-forms-item label="商品名" required name="title">
						<uni-easyinput v-model="customFormData.title" placeholder="请输入商品名" />
					</uni-forms-item>
					<uni-forms-item label="商品描述" required name="description">
						<uni-easyinput v-model="customFormData.description" placeholder="请输入商品描述" />
					</uni-forms-item>
					<uni-forms-item label="价格" required name="price">
						<uni-easyinput v-model="customFormData.price" placeholder="请输入价格" />
					</uni-forms-item>
					<uni-forms-item label="图片" required name="imgUrl">
						<uni-easyinput v-model="customFormData.imgUrl" placeholder="请输入图片" />
					</uni-forms-item>
					<uni-forms-item label="库存" required name="stock">
						<uni-easyinput v-model="customFormData.stock" placeholder="请输入库存" />
					</uni-forms-item>
				</uni-forms>
				<view class="button-group">
					<button type="primary" size="mini" @click="createItem('customForm')">提交创建</button>
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
					description: '',
					title: '',
					stock: '',
					price: '',
					imgUrl: ''
				},
				returnUrl: '/pages/listitem/listitem',
				// 自定义表单校验规则
				customRules: {
					description: {
						rules: [{
							required: true,
							errorMessage: '商品描述不能为空'
						}]
					},
					price: {
						rules: [{
							required: true,
							errorMessage: '价格不能为空'
						}]
					},
					title: {
						rules: [{
							required: true,
							errorMessage: '商品名不能为空'
						}]
					},
					stock: {
						rules: [{
							required: true,
							errorMessage: '库存不能为空'
						}]
					},
					imgUrl: {
						rules: [{
							required: true,
							errorMessage: '图片不能为空'
						}]
					}
				}
			}
		},
		onLoad() {},
		onReady() {
			// 设置自定义表单校验规则，必须在节点渲染完毕后执行
			this.$refs.customForm.setRules(this.customRules)
		},
		methods: {
			createItem(ref) {
				let returnUrl = this.returnUrl
				this.$refs[ref].validate().then(res => {
					console.log('res:', res);
					uni.request({
						url: 'http://localhost:8090/item/create',
						header: {
							'Content-Type': 'application/json;charset=UTF-8'
						},
						method: 'POST',
						data:{
							description: res.description,
							title: res.title,
							stock: res.stock,
							price: res.price,
							imgUrl: res.imgUrl
						},
						success(res) {
							console.log(res)
							uni.showToast({
								title: `添加商品成功`
							})
							uni.navigateTo({
								url: returnUrl
							})
						}
					})
					
				}).catch(err => {
					console.log('err', err);
				})
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
