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
				weapons: [],
				weaponsType:''
			};
		},
		onLoad() {
			this.weaponsType = 'long-sword';
			// this.removeData('weaponsData');
			this.checkData()
		},
		methods: {
			async getWeapons() {
				console.log("正在获取武器数据");
				try{
					let res = await uni.request({
						url: 'https://mhw-db.com/weapons',
						data: {},
						method: 'GET'
					});
						uni.setStorageSync('weaponsData', res.data);
						console.log('武器本地数据存储成功');
						console.log('本地数据重新检查中!');
						this.checkData();// 再次检查本地数据是否存在
						console.log('本地数据重新检查完成!');
						this.renderingData();// 渲染数据
				} catch(err) {
					console.log("请求失败! " + err)
				}
			},
			// 检查确认数据保存在本地，如果未保存在本地则重新获取并保存
			async checkData() {
				try {
					const value = uni.getStorageSync('weaponsData');
					if (value) {
						console.log('武器本地数据存在!');
						this.renderingData();// 渲染数据
						return true;
					} else {
						console.error("武器本地数据丢失，正在重新获取数据!");
						this.getWeapons();
					}
				} catch (e) {
					console.error('武器数据获取失败');
					return false;
				}
			},
			removeData(storageId) {
				// 同步删除数据
				try {
					uni.removeStorageSync(storageId);
					console.log(storageId + ' - 本地数据删除成功!');
				} catch (e) {
					console.error(storageId + ' - 本地数据删除失败!');
				}
			},
			renderingData() {
				let rdDataArray = [];
				let weaponsArray = [];
				try {
					const value = uni.getStorageSync('weaponsData');
					if (value) {
						console.log('武器本地数据获取成功');
						weaponsArray = value;
						for (var i = 0; i < weaponsArray.length; i++) {
							if (weaponsArray[i].type == this.weaponsType) {
								rdDataArray.push(weaponsArray[i])
							}
						}
						this.weapons = rdDataArray;
						console.log(this.weapons);
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
