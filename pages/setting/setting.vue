<template>
	<view>
		<view class="clear-data-btn" @click="removeData()">
			Clean up all local caches
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			// 删除本地数据
			removeData(storageId) {
				// 同步删除数据
				try {
					uni.clearStorageSync();
					console.log('Clear Complete!');
				} catch (err) {
					console.error('Clear fail!' + err);
				}
			},
		}
	}
</script>

<style lang="scss">

</style>
