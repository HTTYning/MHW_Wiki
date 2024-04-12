<template>
	<view>
		<view class="search-box">
			<view class="search-text">
				<input class="search-ipt" @input="searchEvent()" type="text" v-model="searchTxt" placeholder="Please enter a name">
			</view>
			<view class="search-reset-btn" @click="searchReset()">
				<image class="search-reset-icon" src="../../static/weapon/reset.png" mode="aspectFit"></image>
			</view>
		</view>
		<view class="weapon-title-type">
			<view class="weapon-title-type-image">
				<image :src="weaponTypeImagePath" mode="aspectFit"></image>
			</view>
			<view>
				{{weaponsType}}
			</view>
		</view>
		<view class="weaponList">
			<view class="weapon-item" v-for="item in weapons" :key="item.id" @click="weaponDetails(item.id)">
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
						<!-- rarity -->
						<view class="weapon-value-box weapon-rarity">
							Rare:
							<view class="weapon-value">
								{{item.rarity}}
							</view>
						</view>
						<!-- attack -->
						<view class="weapon-value-box weapon-attack">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/attack.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.attack.display}}({{item.attack.raw}})
							</view>
						</view>
						<!-- affinity -->
						<view class="weapon-value-box weapon-affinity">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/affinity.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{(item.attributes.affinity?item.attributes.affinity:'0')+'%'}}
							</view>
						</view>
						<!-- defense -->
						<view class="weapon-value-box weapon-defense">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/defense.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.attributes.defense?item.attributes.defense:'0'}}
							</view>
						</view>
						<!-- elementsDamage and elementsDamageType -->
						<view class="weapon-value-box weapon-elements" v-if="item.elements" v-for="elements in item.elements" :key="item.index">
							<view class="weapon-value-icon">
								<image :src="'../../static/weapon/element/'+elements.type+'.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{elements.damage}}
							</view>
						</view>
						<!-- elderseal -->
						<view class="weapon-value-box weapon-elderseal" v-if="item.elderseal">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/elderseal.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.elderseal}}
							</view>
						</view>
						<!-- slots -->
						<view class="weapon-value-box weapon-slots">
							<view class="weapon-value-icon weapon-slots-icon">
								<image src="../../static/weapon/slots.png" mode="aspectFit"></image>
							</view>
							<image :src="'../../static/weapon/slot-'+(item.slots[0].rank ? item.slots[0].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(item.slots[1].rank ? item.slots[1].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(item.slots[2].rank ? item.slots[2].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
						</view>
						<!-- bowgun - deviation -->
						<view class="weapon-value-box weapon-deviation" v-if="item.deviation">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/deviation.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.deviation}}
							</view>
						</view>
						<!-- durability -->
						<view class="weapon-value-box weapon-durability" v-if="item.durability">
							<view class="weapon-value-icon weapon-durability-icon">
								<image src="../../static/weapon/durability.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-dur-bar">
								<view class="weapon-dur-item" :style="{backgroundColor: 'red', width:((item.durability[0].red/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].red!=0">{{item.durability[0].red}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'orange', width:((item.durability[0].orange/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].orange!=0">{{item.durability[0].orange}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'yellow', width:((item.durability[0].yellow/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].yellow!=0">{{item.durability[0].yellow}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'green', width:((item.durability[0].green/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].green!=0">{{item.durability[0].green}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'blue', width:((item.durability[0].blue/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].blue!=0">{{item.durability[0].blue}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'white', width:((item.durability[0].white/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].white!=0">{{item.durability[0].white}}</view>
								</view>
								<view class="weapon-dur-item" :style="{backgroundColor: 'purple', width:((item.durability[0].purple/400)*100)+'%'}">
									<view class="weapon-dur-value" v-if="item.durability[0].purple!=0">{{item.durability[0].purple}}</view>
								</view>
							</view>
						</view>
						<!-- 5匠珠效果渲染 -->
						<!-- <view class="weapon-durability" v-for="(dur,durIndex) in item.durability" :key="durIndex">
							<view class="weapon-dur-bar" :style="{backgroundColor: 'red', width:((dur.red/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.red!=0">{{dur.red}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'orange', width:((dur.orange/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.orange!=0">{{dur.orange}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'yellow', width:((dur.yellow/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.yellow!=0">{{dur.yellow}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'green', width:((dur.green/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.green!=0">{{dur.green}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'blue', width:((dur.blue/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.blue!=0">{{dur.blue}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'white', width:((dur.white/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.white!=0">{{dur.white}}</view>
							</view>
							<view class="weapon-dur-bar" :style="{backgroundColor: 'purple', width:((dur.purple/400)*100)+'%'}">
								<view class="weapon-dur-value" v-if="dur.purple!=0">{{dur.purple}}</view>
							</view>
						</view> -->
						<!-- bowgun - ammo -->
						<view class="weapon-value-box weapon-ammo" v-if="item.ammo">
							<view class="weapon-ammo-item" v-for="(ammo,ammoIndex) in item.ammo" :key="ammoIndex">
								<view class="weapon-value-icon weapon-ammo-icon">
									<image :src="'../../static/weapon/ammo/'+ammo.type+'.png'" mode="aspectFit"></image>
								</view>
								<view class="weapon-value weapon-ammo-number" v-for="capacities in ammo.capacities" :key="capacities.index">
									{{capacities}}
								</view>
							</view>
						</view>
						<!-- bowgun - specialAmmo -->
						<view class="weapon-value-box weapon-specialAmmo" v-if="item.specialAmmo">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/specialAmmo.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.specialAmmo}}
							</view>
						</view>
						<!-- bow - coatings -->
						<view class="weapon-value-box weapon-coatings" v-if="item.coatings">
							<view class="weapon-value-icon weapon-coatings-icon" v-for="(coatings,coatingsIndex) in item.coatings" :key="coatingsIndex">
								<image :src="'../../static/weapon/coatings/'+coatings+'.png'" mode="aspectFit"></image>
							</view>
						</view>
						<!-- gunlance - shelling -->
						<view class="weapon-value-box weapon-shelling" v-if="item.shelling">
							<view class="weapon-value-icon weapon-shelling-icon">
								<image src="../../static/weapon/shelling.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.shelling.type}}
							</view>
							<view class="weapon-value">
								Level{{item.shelling.level}}
							</view>
						</view>
						<!-- switch-axe - phial -->
						<view class="weapon-value-box weapon-phial" v-if="item.phial">
							<view class="weapon-value-icon weapon-phial-icon">
								<image :src="'../../static/weapon/phial.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.phial.type}}
							</view>
							<!-- <view class="weapon-value">
								{{item.phial.damage}}
							</view> -->
						</view>
						<!-- insect-glaive - boostType and damageType -->
						<view class="weapon-value-box weapon-boostType" v-if="item.boostType">
							<view class="weapon-value-icon weapon-boostType-icon">
								<image :src="'../../static/weapon/boostType.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value">
								{{item.boostType}}{{item.damageType != item.boostType ? '【'+item.damageType+'】' : ''}}
							</view>
						</view>
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
				weaponsType:'',
				weaponTypeImagePath:'',
				searchTxt:''
			};
		},
		onLoad(option) {
			this.weaponsType = option.weaponName;
			this.weaponTypeImagePath = '../../static/index-weapon/weapon_type_'+this.weaponsType+'.png'
			// this.removeData('weaponsData');
			this.checkData();
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
			// 删除本地数据（未启用）
			removeData(storageId) {
				// 同步删除数据
				try {
					uni.removeStorageSync(storageId);
					console.log(storageId + ' - 本地数据删除成功!');
				} catch (e) {
					console.error(storageId + ' - 本地数据删除失败!');
				}
			},
			// 加载数据到数组里进行数据渲染
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
			},
			// 清空搜索框
			searchReset(){
				this.searchTxt = '';
				this.searchEvent();
			},
			// 搜索事件
			searchEvent(){
				let tempArray = [];
				let weaponsArray = [];
				// 创建一个正则表达式，添加 'i' 标志来执行不区分大小写的匹配  
				let regex = new RegExp(this.searchTxt, 'i');  
				if(this.searchTxt != ''){
					const value = uni.getStorageSync('weaponsData');
					if (value) {
						weaponsArray = value;
						for (var i = 0; i < weaponsArray.length; i++) {
							if (weaponsArray[i].type == this.weaponsType && regex.test(weaponsArray[i].name)) {
								tempArray.push(weaponsArray[i])
							}
						}
						this.weapons = tempArray;
					}
				}else{
					this.checkData();
				}
			},
			// 跳转到武器详情页面
			weaponDetails(weaponId){
				uni.navigateTo({
					url: '/pages/weaponDetails/weaponDetails?&weaponId='+weaponId
				});
			},
		}
	}
</script>

<style lang="scss">
	.search-box{
		width: 80%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		position: fixed;
		bottom: 30px;
		left: 50%;
		transform: translateX(-50%);
		border: 2px solid rgba(180, 180, 180, 1);
		border-radius: 5px;
		overflow: hidden;
		z-index: 1000;
		.search-text{
			width: 100%;
			.search-ipt{
				height: 40px;
				padding: 0 26px 0 6px;
				background-color: rgba(240, 240, 240, 1);
			}
		}
		.search-reset-btn{
			width: 20px;
			height: 40px;
			position: absolute;
			right: 0px;
			top: 50%;
			transform: translateY(-50%);
			display: flex;
			justify-content: center;
			align-items: center;
			padding-right: 5px;
			image{
				width: 20px;
				height: 20px;
				transform: rotate(45deg);
			}
		}
	}
	.weapon-title-type{
		width: 100%;
		height: 40px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		font-weight: bold;
		font-size: 22px;
		background-color: rgba(240, 240, 240, 1);
		color: rgba(80, 80, 80, 1);
		border-bottom: 2px solid rgba(200, 200, 200, 1);
		.weapon-title-type-image{
			width: 26px;
			height: 26px;
			margin-right: 10px;
			image{
				width: 100%;
				height: 100%;
			}
		}
	}
	.weaponList {
		.weapon-item{
			width: 100%;
			display: flex;
			flex-direction: column;
			box-sizing: border-box;
			margin-bottom: 5px;
			border-top: 2px solid rgba(200, 200, 200, 1);
			.weapon-titile-bar{
				height: 30px;
				display: flex;
				flex-direction: row;
				justify-content: left;
				align-items: center;
				padding: 5px;
				border-bottom: 2px solid rgba(223, 223, 223, 1);
				border-top: 1px solid rgba(223, 223, 223, 1);
				background-color: rgba(247, 247, 247, 1);
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
					color: rgba(45, 45, 45, 1);
				}
			}
			.weapon-attribute{
				display: flex;
				flex-direction: row;
				align-items: center;
				padding: 5px 0;
				.weapon-img{
					width: 100px;
					height: 100px;
					flex-shrink: 0;
					image{
						width: 100%;
						height: 100%;
					}
				}
				.weapon-attribute-value{
					margin: 3px 0;
					.weapon-value-box{
						display: flex;
						flex-direction: row;
						align-items: center;
						justify-content: left;
						margin-bottom: 2px;
						.weapon-value-icon{
							width: 18px;
							height: 18px;
							margin-right: 3px;
							background-color: rgba(220, 220, 220, 1);
							border-radius: 3px;
							padding: 2px;
							image{
								width: 100%;
								height: 100%;
							}
						}
						.weapon-value{
							color: rgba(80, 80, 80, 1);
							margin-right: 3px;
						}
					}
					.weapon-slots{
						display: flex;
						flex-direction: row;
						>image{
							width: 20px;
							height: 20px;
						}
					}
					.weapon-durability{
						.weapon-dur-bar{
							width: 200px;
							height: 18px;
							display: flex;
							flex-direction: row;
							align-items: center;
							background-color: rgba(200, 200, 200, 1);
							border: 1px solid rgba(180, 180, 180, 1);
							border-radius: 1px;
							.weapon-dur-item{
								height: 100%;
								opacity: 0.8;
								position: relative;
								.weapon-dur-value{
									position: absolute;
									top: 50%;
									left: 50%;
									transform: translate(-50%,-50%);
									color: black;
									font-size: 8px;
									text-shadow: -1px 0 rgba(200, 200, 200, 1),  /* 左边 */  
									             0 1px rgba(200, 200, 200, 1),  /* 下边 */  
									             1px 0 rgba(200, 200, 200, 1),  /* 右边 */  
									             0 -1px rgba(200, 200, 200, 1); /* 上边 */  
								}
							}
						}
					}
					.weapon-ammo{
						display: flex;
						flex-direction: row;
						flex-wrap: wrap;
						.weapon-ammo-item{
							display: flex;
							justify-content: left;
							flex: 0 0 calc(31%);
							box-sizing: border-box;
							align-items: center;
							border: 1px solid rgba(223, 223, 223, 1);
							margin-right: 1px;
							margin-bottom: 1px;
							margin-top: 0;
							border-radius: 3px;
							.weapon-ammo-number{
								padding: 1px;
							}
						}
					}
					.weapon-coatings{
						display: flex;
						flex-direction: row;
						.weapon-coatings-icon{
							width: 22px;
							height: 22px;
							background-color: rgba(240, 240,240, 1);
							border-radius: 5px;
						}
					}
				}
			}
		}
	}
</style>
