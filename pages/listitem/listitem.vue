<template>
	<view>
		<view class="uni-container">
			<uni-section title="搜索框" type="line">
				<uni-search-bar @confirm="search" :focus="true" v-model="searchValue" @input="input" @cancel="cancel"
					@clear="clear">
				</uni-search-bar>
			</uni-section>
			<uni-section title="添加活动商品" type="line">
				<button class="uni-button" size="mini" type="primary" @click="tocreateitem()">添加</button>
			</uni-section>
			<uni-section title="活动商品列表" type="line">
				<uni-table ref="table" :loading="loading" emptyText="暂无更多数据">
					<uni-tr>
						<uni-th width="80" align="center">商品名</uni-th>
						<uni-th width="100" align="center">商品图片</uni-th>
						<uni-th width="300" align="center">商品描述</uni-th>
						<uni-th width="80" align="center">商品价格</uni-th>
						<uni-th width="40" align="center">商品库存</uni-th>
						<uni-th width="40" align="center">商品销量</uni-th>
						<uni-th width="100" align="center">设置</uni-th>
					</uni-tr>
					<uni-tr v-for="(item, index) in tableData" :key="index">
						<uni-td>
							<view class="name">{{ item.title }}</view>
						</uni-td>
						<uni-td>
							<image :src="getsrc(item.imgUrl)" />
						</uni-td>
						<uni-td align="center">{{ item.description }}</uni-td>
						<uni-td>{{ item.price }}</uni-td>
						<uni-td>{{ item.stock }}</uni-td>
						<uni-td>{{ item.sales }}</uni-td>
						<uni-td>
							<view class="uni-group">
								<button class="uni-button" size="mini" type="primary"
									@click="getitem(item.idStr)">详情</button>
							</view>
						</uni-td>
					</uni-tr>
				</uni-table>
				<view class="uni-pagination-box">
					<uni-pagination show-icon :page-size="pageSize" :current="pageCurrent" :total="total" />
				</view>
			</uni-section>
		</view>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				searchVal: '',
				getitemurl: '/pages/getitem/getitem',
				createitem: '/pages/createitem/createitem',
				searchValue: '输入商品名',
				tableData: [{
					"description": "联想拯救者Y7000P 2022 英特尔酷睿i5 15.6英寸游戏笔记本电脑(12代i5-12500H 16G 512G RTX3050 2.5k电竞屏)",
					"imgUrl": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ0fYHgzv0IJWDWX3YKxaEuepxdq0FjF-9J0U66FfSrjg&s",
					"price": 7299.2,
					"promo": {
						"endDate": "2022-09-10T08:51:25.878Z",
						"promoItemPrice": 6299.2,
						"promoName": "联想拯救者Y7000P 1000折扣卷",
						"startDate": "2022-09-07T08:51:25.878Z",
						"status": 1
					},
					"sales": 0,
					"stock": 100,
					"title": "联想拯救者Y7000P",
					"idStr": "763404919783817216"
				}],
				// 每页数据量
				pageSize: 2,
				// 当前页
				pageCurrent: 1,
				// 数据总量
				total: 1,
				loading: false
			}
		},
		onLoad() {
			console.log("onloading")
		},
		methods: {
			getsrc(url) {
				return url;
			},
			// 获取数据
			getData(pageCurrent, value = '') {
				this.loading = true
				this.pageCurrent = pageCurrent
				this.request({
					pageSize: this.pageSize,
					pageCurrent: pageCurrent,
					value: value,
					success: res => {
						// console.log('data', res);
						this.tableData = res.data
						this.total = res.total
						this.loading = false
					}
				})
			},
			getitem(id) {
				console.log(id)
				uni.navigateTo({
					url: this.getitemurl + "?id=" + id
				})
			},
			tocreateitem(){
				uni.navigateTo({
					url: this.createitem
				})
			},
			search(res) {
				uni.showToast({
					title: '搜索：' + res.value,
					icon: 'none'
				})
			},
			input(res) {
				console.log('----input:', res)
			},
			cancel(res) {
				uni.showToast({
					title: '点击取消，输入值为：' + res.value,
					icon: 'none'
				})
			}
		}
	}
</script>


<style>
	.uni-group {
		display: flex;
		align-items: center;
	}

	.search-result {
		padding-top: 10px;
		padding-bottom: 20px;
		text-align: center;
	}

	.search-result-text {
		text-align: center;
		font-size: 14px;
		color: #666;
	}

	.example-body {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		padding: 0px;
	}

	.uni-mt-10 {
		margin-top: 10px;
	}
</style>
