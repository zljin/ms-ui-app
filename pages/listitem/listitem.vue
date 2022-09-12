<template>
	<view>
		<view class="uni-container">
			<uni-section title="搜索框" type="line">
				<uni-search-bar @confirm="search" :focus="true" v-model="searchValue" @input="input" @cancel="cancel"
					@clear="clear">
				</uni-search-bar>
			</uni-section>
			<uni-section title="添加活动商品" type="line">
				<button class="uni-button" size="mini" type="primary" @click="toCreateItem()">添加</button>
			</uni-section>
			<uni-section title="活动商品列表" type="line">
				<uni-table ref="table" :loading="loading" border stripe type="selection" emptyText="暂无更多数据" @selection-change="selectionChange">
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
							<image :src="getSrc(item.imgUrl)" />
						</uni-td>
						<uni-td align="center">{{ item.description }}</uni-td>
						<uni-td>{{ item.price }}</uni-td>
						<uni-td>{{ item.stock }}</uni-td>
						<uni-td>{{ item.sales }}</uni-td>
						<uni-td>
							<view class="uni-group">
								<button class="uni-button" size="mini" type="primary"
									@click="getItem(item.idStr)">详情</button>
							</view>
						</uni-td>
					</uni-tr>
				</uni-table>
			<view class="uni-pagination-box"><uni-pagination show-icon :page-size="pageSize" :current="pageCurrent" :total="total" @change="change" /></view>
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
				tableData: [],
				// 每页数据量
				pageSize: 3,
				// 当前页
				pageCurrent: 1,
				// 数据总量
				total: 0,
				loading: false
			}
		},
		onLoad() {
			console.log("onloading")
			this.getData(1)
		},
		methods: {
			getSrc(url) {
				return url;
			},
			// 获取数据
			getData(pageCurrent) {
				this.loading = true
				let that = this
				uni.request({
					url: 'http://localhost:8090/item/get-list',
					method: "GET",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					data: {
						pageCurrent: pageCurrent,
						pageSize: that.pageSize,
						title: that.searchVal
					},
					success(res) {
						console.log(res)
						if (res.data.status == "success") {
							that.tableData = res.data.data
							that.total = res.data.total
						}
						that.loading = false
					}
				})
			},
			getItem(id) {
				console.log(id)
				uni.navigateTo({
					url: this.getitemurl + "?id=" + id
				})
			},
			selectionChange(e) {
				console.log(e.detail.index)
				this.selectedIndexs = e.detail.index
			},
			// 分页触发
			change(e) {
				this.$refs.table.clearSelection()
				this.selectedIndexs.length = 0
				this.getData(e.current)
			},
			toCreateItem() {
				uni.navigateTo({
					url: this.createitem
				})
			},
			search(res) {
				this.searchVal = res.value
				this.getData()
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
