<template>
	<view class="container">
		<uni-section title="商品详情" type="line">
			<uni-card title="商品下单" padding="20px 0">
				<image style="width: 100%;" :src="product.imgUrl"></image><br>
				<text class="uni-body uni-mt-5">商品名称：{{ product.title }}</text><br>
				<text class="uni-body uni-mt-5">商品描述：{{ product.description }}</text><br>
				<text class="uni-body uni-mt-5">商品库存：{{ product.stock }}</text><br>
				<text class="uni-body uni-mt-5">商品价格：{{ product.price }}</text><br>
				<text class="uni-body uni-mt-5">促销状态：{{ product.promoStatusStr }}</text><br>
				<text class="uni-body uni-mt-5">促销名称：{{ product.promoName }}</text><br>
				<text class="uni-body uni-mt-5">促销开始时间：{{ product.startDate }}</text><br>
				<text class="uni-body uni-mt-5">促销结束时间：{{ product.endDate }}</text><br>
				<text class="uni-body uni-mt-5">促销后的价格：{{ product.promoItemPrice }}</text><br>
				购买数量：
				<uni-easyinput v-model="amount" placeholder="请输入购买数量" /><br>
				<view class="button-group">
					<button type="primary" @click="createOrder()">下单</button>
				</view>
			</uni-card>
		</uni-section>
	</view>
</template>


<script>
	import helper from '../../common/helper.js';
	export default {
		data() {
			return {
				product: {
					id: 0,
					idStr: '',
					title: '',
					description: '',
					stock: 0,
					price: '',
					sales: '',
					imgUrl: '',
					startDate: '',
					endDate: '',
					promoId: '',
					promoName: '',
					promoItemPrice: '',
					promoStatus: 0,
					promoStatusStr: '无秒杀活动'
				},
				amount: 0,
				timer: null
			}
		},
		onLoad() {
			console.log("onloading")
			console.log("token:" + helper.token)
			let id = this.$route.query.id
			this.product.id = id
			console.log(id)
			this.getItem(id)
			this.reloadDom(id)
		},
		onReady() {},
		onUnload() {
			if(this.timer) {
				clearTimeout(this.timer);  
				this.timer = null;  
			}  
		},
		methods: {
			reloadDom(id) {
				this.timer = setInterval( () => {
				    this.getItem(id)	
				}, 1000)
			},
			getItem(id) {
				let that = this
				uni.request({
					url: 'http://localhost:8090/item/get',
					method: "GET",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					data: {
						id: id
					},
					success(res) {
						console.log(res)
						if (res.data.status == "success") {
							that.product = res.data.data
							if (that.product.promoStatus == 2) {
								that.product.promoStatusStr = '秒杀活动进行中'
							} else if (that.product.promoStatus == 1) {
								that.product.promoStatusStr = '秒杀活动等待开始'
							} else {
								that.product.promoStatusStr = '无秒杀活动'
							}
						}
					}
				})
			},
			createOrder() {
				let that = this
				uni.request({
					url: 'http://localhost:8090/order/createorder',
					method: "POST",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded',
						'token': helper.token
					},
					data: {
						itemId: this.product.idStr,
						amount: this.amount,
						promoId: this.product.promoId
					},
					success(res) {
						console.log(res)
						if (res.data.status == "success") {
							uni.showToast({
								title: `下单成功，订单号：` + res.data.data.id
							})
						}
					}
				})
			}
		}
	}
</script>


<style lang="scss">

</style>
