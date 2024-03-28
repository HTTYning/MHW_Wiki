<template>
	<view>
		<view class="weaponList">
			<view>一共 {{weapons.length}} 把太刀</view>
			<view class="weapon-item" v-for="item in weapons" :key="item.id">
				<view>{{item.id}} - {{item.name}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				weapons: []
			};
		},
		onLoad() {
			// this.getWeapons();
			// 同步获取数据
			// try {
			// 	const value = uni.getStorageSync('weaponsData');
			// 	if (value) {
			// 		console.log('武器数据获取成功');
			// 		console.log(value);
			// 		this.weapons = value;
			// 	}
			// } catch (e) {
			// 	console.error('武器数据获取失败');
			// }
			this.getLong();
		},
		methods: {
			getWeapons() {
				uni.request({
					url: 'https://mhw-db.com/weapons',
					data: {},
					method: 'GET',
					success: (res) => {
						// console.log(res.data);
						// console.log(res.data[0]);

						// 同步存储数据
						try {
							uni.setStorageSync('weaponsData', res.data);
							console.log('武器数据保存成功');
						} catch (e) {
							console.error('武器数据保存失败');
						}
						// 同步获取数据
						try {
							const value = uni.getStorageSync('weaponsData');
							if (value) {
								console.log('武器数据获取成功');
								console.log(value);
								this.weapons = value;
							}
						} catch (e) {
							console.error('武器数据获取失败');
						}
						// 同步删除数据
						// try {
						// 	uni.removeStorageSync('storage_key');
						// } catch (e) {
						// 	// error
						// }
					}
				});
			},
			getLong(){
				let longs = [];
				let weaponsArray = [];
				try {
					const value = uni.getStorageSync('weaponsData');
					if (value) {
						console.log('武器数据获取成功');
						weaponsArray = value;
						for (var i = 0; i < weaponsArray.length; i++) {
							if(weaponsArray[i].type == "long-sword"){
								longs.push(weaponsArray[i])
							}
						}
						this.weapons = longs;
					}
				} catch (e) {
					console.error('武器数据获取失败');
				}
			}
		}
	}
</script>

<style lang="scss">
	.weaponList {}
</style>
