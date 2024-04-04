<template>
	<view>
		<view class="weaponList">
			<view class="weapon-number">{{weaponsType}} / {{weapons.length}}</view>
			<view class="weapon-item" v-for="item in weapons" :key="item.id">
				<view class="weapon-titile-bar">
					<view class="weapon-icon">
						<img :src="item.assets.icon" alt="" mode="aspectFit">
					</view>
					<view class="weapon-name">{{item.name}}</view>
				</view>
				<view class="weapon-attribute">
					<view class="weapon-img">
						<img :src="item.assets.image" alt="" mode="aspectFit">
					</view>
					<view class="weapon-attribute-value">
						<view class="weapon-rarity">
							rarity: {{item.rarity}}
						</view>
						<view class="weapon-attack">
							attack: {{item.attack.display}}({{item.attack.raw}})
						</view>
						<view class="weapon-affinity">
							affinity: {{(item.attributes.affinity?item.attributes.affinity:'0')+'%'}}
						</view>
						<view class="weapon-elements" v-if="item.elements" v-for="elements in item.elements" :key="item.index">
							elements: {{elements.type}} {{elements.damage}}
							<br>
							{{item.elderseal ? "elderseal: "+item.elderseal:""}}
						</view>
						<view class="weapon-slots">
							<image :src="'../../static/weapon/slot-'+(item.slots[0].rank ? item.slots[0].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(item.slots[1].rank ? item.slots[1].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(item.slots[2].rank ? item.slots[2].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
						</view>
						<view class="weapon-durability" v-if="item.durability">
							<view :style="{backgroundColor: 'red', width:((item.durability[0].red/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'orange', width:((item.durability[0].orange/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'yellow', width:((item.durability[0].yellow/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'green', width:((item.durability[0].green/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'blue', width:((item.durability[0].blue/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'white', width:((item.durability[0].white/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'purple', width:((item.durability[0].purple/400)*100)+'%'}"></view>
						</view>
						<!-- <view class="weapon-durability" v-for="(dur,durIndex) in item.durability" :key="durIndex">
							<view :style="{backgroundColor: 'red', width:((dur.red/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'orange', width:((dur.orange/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'yellow', width:((dur.yellow/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'green', width:((dur.green/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'blue', width:((dur.blue/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'white', width:((dur.white/400)*100)+'%'}"></view>
							<view :style="{backgroundColor: 'purple', width:((dur.purple/400)*100)+'%'}"></view>
						</view> -->
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				weapons: [],
				slotsNullStr:"null",
				weaponsType:''
			};
		},
		onLoad(option) {
			this.weaponsType = option.weaponName;
			// this.weaponsType = 'long-sword';
			// this.weaponsType = 'great-sword';
			// this.weaponsType = 'light-bowgun';
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
						console.log(value);
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
	.weapon-number{
		padding: 5px;
		font-size: 22px;
		font-weight: bold;
		background-color: rgba(0, 0, 0, .2);
	}
	.weaponList {
		.weapon-item{
			width: 100%;
			display: flex;
			flex-direction: column;
			box-sizing: border-box;
			margin-bottom: 5px;
			border-top: 2px solid gray;
			.weapon-titile-bar{
				height: 30px;
				display: flex;
				flex-direction: row;
				justify-content: left;
				align-items: center;
				padding: 5px;
				border-bottom: 2px solid rgba(0, 0, 0, .1);
				.weapon-icon{
					width: 30px;
					height: 30px;
					image{
						width: 100%;
						height: 100%;
					}
				}
				.weapon-name{
					margin-left: 10px;
					size: 20px;
					font-weight: bold;
					color:black;
				}
			}
			.weapon-attribute{
				display: flex;
				flex-direction: row;
				align-items: center;
				padding: 5px 0;
				view{
					margin: 3px 0;
				}
				.weapon-img{
					width: 100px;
					height: 100px;
					image{
						width: 100%;
						height: 100%;
					}
				}
				.weapon-slots{
					display: flex;
					flex-direction: row;
					image{
						width: 20px;
						height: 20px;
					}
				}
				.weapon-durability{
					width: 150px;
					height: 10px;
					display: flex;
					flex-direction: row;
					align-items: center;
					background-color: gray;
					border: 1px solid black;
					view{
						height: 100%;
					}
				}
			}
		}
	}
</style>
