<template>
	<view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		onLoad() {
			console.log("onloading")
			let id = this.$route.query.id
			console.log(id)
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
