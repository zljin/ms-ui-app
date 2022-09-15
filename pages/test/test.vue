<template>
	<view class="container">
		<uni-section title="自定义卡片内容" type="line">
			<uni-card title="基础卡片" sub-title="副标题" extra="额外信息" padding="20px 0">
				<template v-slot:title>
					<uni-list>
						<uni-list-item :show-switch="true" title="自定义标题" />
					</uni-list>
				</template>
				<image style="width: 100%;" :src="cover"></image><br>
				<text class="uni-body uni-mt-5">卡片组件通用来显</text>
			</uni-card>
		</uni-section>
	</view>
</template>


<script>
	export default {
		components: {},
		data() {
			return {
				cover: 'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/094a9dc0-50c0-11eb-b680-7980c8a877b8.jpg'
			}
		},
		methods: {
		}
	}
</script>
