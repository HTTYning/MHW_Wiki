<template>
	<view>
		<view class="weapon-content">
			<view class="weapon-titile">
				<view class="weapon-icon">
					<img :src="weaponObj.assets.icon" alt="" mode="aspectFit">
				</view>
				<view class="weapon-name">{{weaponObj.name}}</view>
			</view>
			<view class="weapon-image-box">
				<image class="weapon-image" :src="weaponObj.assets.image" mode="aspectFit"></image>
			</view>
			<view class="weapon-attribute-box">
				<view class="weapon-attribute-title">
					Attribute
				</view>
				<view class="weapon-attribute-value">
					<!-- rarity -->
					<view class="weapon-value-box weapon-rarity">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image :src="weaponObj.assets.icon" mode="aspectFit">
							</view>
							<view class="weapon-value-name">
								{{weaponObj.name}}
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								Rare {{weaponObj.rarity}}
							</view>
						</view>
					</view>
					<!-- attack -->
					<view class="weapon-value-box weapon-attack-box">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/attack.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								attack
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.attack.display}}({{weaponObj.attack.raw}})
							</view>
						</view>
					</view>
					<!-- affinity -->
					<view class="weapon-value-box weapon-affinity">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/affinity.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								affinity
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{(weaponObj.attributes.affinity?weaponObj.attributes.affinity:'0')+'%'}}
							</view>
						</view>
					</view>
					<!-- defense -->
					<view class="weapon-value-box weapon-defense">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/defense.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								defense
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.attributes.defense?weaponObj.attributes.defense:'0'}}
							</view>
						</view>
					</view>
					<!-- elementsDamage and elementsDamageType -->
					<view class="weapon-value-box weapon-elements" v-for="elements in weaponObj.elements" :key="item.index">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image :src="'../../static/weapon/element/'+elements.type+'.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								damage
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{elements.damage}}
							</view>
						</view>
					</view>
					<!-- elderseal -->
					<view class="weapon-value-box weapon-elderseal" v-if="weaponObj.elderseal">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/elderseal.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								elderseal
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.elderseal}}
							</view>
						</view>
					</view>
					<!-- slots -->
					<view class="weapon-value-box weapon-slots">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon weapon-slots-icon">
								<image src="../../static/weapon/slots.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								slots
							</view>
						</view>
						<view class="weapon-value-layout-right weapon-slots-value">
							<image :src="'../../static/weapon/slot-'+(weaponObj.slots[0].rank ? weaponObj.slots[0].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(weaponObj.slots[1].rank ? weaponObj.slots[1].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
							<image :src="'../../static/weapon/slot-'+(weaponObj.slots[2].rank ? weaponObj.slots[2].rank : slotsNullStr)+'.png'" mode="aspectFit"></image>
						</view>
					</view>
					<!-- bowgun - deviation -->
					<view class="weapon-value-box weapon-deviation" v-if="weaponObj.deviation">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/deviation.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								elderseal
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.deviation}}
							</view>
						</view>
					</view>
					<!-- durability -->
					<view class="weapon-value-box weapon-durability" @click="checkWeaponnDurability()" :class="weaponDurabilityStatus ? 'weapon-durability-show':''" v-if="weaponObj.durability">
						<view class="weapon-durability-box-top">
							<view class="weapon-value-layout-left">
								<view class="weapon-value-icon weapon-durability-icon">
									<image src="../../static/weapon/durability.png" mode="aspectFit"></image>
								</view>
								<view class="weapon-value-name">
									durability
								</view>
							</view>
							<view class="weapon-value-layout-right weapon-durability-bar-box">
								<view class="weapon-durability-bar">
									<view class="weapon-durability-item" :style="{backgroundColor: 'red', width:((weaponObj.durability[0].red/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'orange', width:((weaponObj.durability[0].orange/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'yellow', width:((weaponObj.durability[0].yellow/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'green', width:((weaponObj.durability[0].green/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'blue', width:((weaponObj.durability[0].blue/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'white', width:((weaponObj.durability[0].white/400)*100)+'%'}"></view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'purple', width:((weaponObj.durability[0].purple/400)*100)+'%'}"></view>
								</view>
								<view class="weapon-durability-btn" :class="weaponDurabilityStatus ? 'weapon-durability-btn-show':''">
									<image src="../../static/weapon/show.png" mode="aspectFit"></image>
								</view>
							</view>
						</view>
						<view class="weapon-durability-box-bottom">
							<view class="weapon-durability-group" v-for="(durabilityItem,durabilityItemIndex) in weaponObj.durability" :key="durabilityItemIndex">
								<view class="weapon-durability-level">
									{{durabilityItemIndex}}
								</view>
								<view class="weapon-durability-group-bar">
									<view class="weapon-durability-item" :style="{backgroundColor: 'red', width:((durabilityItem.red/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.red!=0">{{durabilityItem.red}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'orange', width:((durabilityItem.orange/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.orange!=0">{{durabilityItem.orange}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'yellow', width:((durabilityItem.yellow/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.yellow!=0">{{durabilityItem.yellow}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'green', width:((durabilityItem.green/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.green!=0">{{durabilityItem.green}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'blue', width:((durabilityItem.blue/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.blue!=0">{{durabilityItem.blue}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'white', width:((durabilityItem.white/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.white!=0">{{durabilityItem.white}}</view>
									</view>
									<view class="weapon-durability-item" :style="{backgroundColor: 'purple', width:((durabilityItem.purple/400)*100)+'%'}">
										<view class="weapon-durability-value" v-if="durabilityItem.purple!=0">{{durabilityItem.purple}}</view>
									</view>
								</view>
							</view>
						</view>
					</view>
					<!-- bowgun - ammo -->
					<view class="weapon-value-box weapon-ammo" v-if="weaponObj.ammo">
						<view class="weapon-ammo-item" v-for="(ammo,ammoIndex) in weaponObj.ammo" :key="ammoIndex">
							<view class="weapon-ammo-icon">
								<image :src="'../../static/weapon/ammo/'+ammo.type+'.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-ammo-number">
								{{ammo.capacities[0] ? ammo.capacities[0]:'0'}}
							</view>
							<view class="weapon-ammo-number">
								{{ammo.capacities[1] ? ammo.capacities[1]:'0'}}
							</view>
							<view class="weapon-ammo-number">
								{{ammo.capacities[2] ? ammo.capacities[2]:'0'}}
							</view>
						</view>
					</view>
					<!-- bowgun - specialAmmo -->
					<view class="weapon-value-box weapon-specialAmmo" v-if="weaponObj.specialAmmo">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon">
								<image src="../../static/weapon/specialAmmo.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								specialAmmo
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.specialAmmo}}
							</view>
						</view>
					</view>
					<!-- bow - coatings -->
					<view class="weapon-value-box weapon-coatings" v-if="weaponObj.coatings">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon weapon-coatings-icon">
								<image :src="'../../static/weapon/coatings/close range.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								Coatings
							</view>
						</view>
						<view class="weapon-value-layout-right weapon-coatings-value-box">
							<view class="weapon-value weapon-coatings-value" v-for="(coatings,coatingsIndex) in weaponObj.coatings" :key="coatingsIndex">
								<view class="weapon-coatings-image">
									<image :src="'../../static/weapon/coatings/'+coatings+'.png'" mode="aspectFit"></image>
								</view>
								<view class="weapon-coatings-name">{{coatings}}</view>
							</view>
						</view>
					</view>
					<!-- gunlance - shelling -->
					<view class="weapon-value-box weapon-shelling" v-if="weaponObj.shelling">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon weapon-shelling-icon">
								<image src="../../static/weapon/shelling.png" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								shelling
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.shelling.type}}
							</view>
							<view class="weapon-value">
								Level{{weaponObj.shelling.level}}
							</view>
						</view>
					</view>
					<!-- switch-axe - phial -->
					<view class="weapon-value-box weapon-phial" v-if="weaponObj.phial">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon weapon-phial-icon">
								<image :src="'../../static/weapon/phial.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								phial
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.phial.type}}
							</view>
							<!-- <view class="weapon-value">
								{{weaponObj.phial.damage}}
							</view> -->
						</view>
					</view>
					<!-- insect-glaive - boostType and damageType -->
					<view class="weapon-value-box weapon-boostType" v-if="weaponObj.boostType">
						<view class="weapon-value-layout-left">
							<view class="weapon-value-icon weapon-boostType-icon">
								<image :src="'../../static/weapon/boostType.png'" mode="aspectFit"></image>
							</view>
							<view class="weapon-value-name">
								boostType
							</view>
						</view>
						<view class="weapon-value-layout-right">
							<view class="weapon-value">
								{{weaponObj.boostType}}{{weaponObj.damageType != weaponObj.boostType ? '【'+weaponObj.damageType+'】' : ''}}
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
				weaponId:null,
				weaponObj:{},
				slotsNullStr:"null",
				weaponDurabilityStatus:false
			};
		},
		onLoad(option){
			// this.weaponId = option.weaponId;
			this.weaponObj = JSON.parse(decodeURIComponent(option.weaponItem));
			
			// console.log(this.weaponId);
			console.log(this.weaponObj);
			// this.getWeaponDetails();
		},
		methods:{
			async getWeaponDetails(){
				try{
					let res = await uni.request({
						url: 'https://mhw-db.com/weapons/'+this.weaponId,
						// url: 'https://mhw-db.com/items',
						data: {},
						method: 'GET'
					});
					console.log(res.data);
					// console.log(this.weaponObj);
					this.weaponObj = res.data;
				} catch(err) {
					console.log("请求失败! " + err)
				}
			},
			checkWeaponnDurability(){
				this.weaponDurabilityStatus = !this.weaponDurabilityStatus;
			},
		}
	}
</script>

<style lang="scss">
	.weapon-content{
		.weapon-titile{
			width: 100%;
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
		.weapon-image-box{
			width: 100%;
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
			.weapon-image{
				width: 150px;
				height: 150px;
			}
		}
		.weapon-attribute-box{
			width: 100%;
			display: flex;
			flex-direction: column;
			align-items: center;
			.weapon-attribute-title{
				width: 100%;
				height: 34px;
				background: linear-gradient(to right, transparent,  rgba(0,0,0,.6), rgba(0,0,0, .8), rgba(0,0,0, .6), transparent);
				color: white;
				text-align:  center;
				line-height: 34px;
				font-size: 18px;
			}
			.weapon-attribute-value{
				width: 100%;
				display: flex;
				flex-direction: column;
				align-items: left;
				padding: 5px 10px;
				box-sizing: border-box;
				color: rgba(45, 45, 45, 1);
				.weapon-value-box{
					width: 100%;
					margin-right: 26px;
					display: flex;
					flex-direction: row;
					justify-content: space-between;
					align-items: center;
					border-bottom: 1px solid rgba(223, 223, 223, 1);
					padding: 1px 0;
					margin: 1px 0;
					.weapon-value-layout-left{
						display: flex;
						flex-direction: row;
						align-items: center;
						flex-shrink: 0;
						.weapon-value-icon{
							width: 26px;
							height: 26px;
							background-color: rgba(45, 45, 45, 1);
							border-radius: 3px;
							display: flex;
							justify-content: center;
							align-items: center;
							image{
								width: 24px;
								height: 24px;
							}
						}
						.weapon-value-name{
							margin-left: 3px;
						}
					}
					.weapon-value-layout-right{
						.weapon-value{
							
						}
					}
				}
				.weapon-slots{
					.weapon-slots-value{
						display: flex;
						flex-direction: row;
						align-items: center;
						image{
							width: 24px;
							height: 24px;
						}
					}
				}
				.weapon-durability{
					max-height: 26px;
					overflow: hidden;
					display: flex;
					flex-direction: column;
					transition: all 0.5s ease-out 0s;
					.weapon-durability-box-top{
						width: 100%;
						display: flex;
						flex-direction: row;
						justify-content: space-between;
						align-items: center;
						.weapon-durability-bar-box{
							display: flex;
							flex-direction: row;
							align-items: center;
							margin-left: 20px;
							flex: 1;
							.weapon-durability-btn{
								display: flex;
								align-items: center;
								flex-shrink: 0;
								margin-left: 3px;
								transition: all 0.2s ease-out 0s;
								image{
									width: 22px;
									height: 22px;
								}
							}
							.weapon-durability-btn-show{
								transform: rotate(180deg);
							}
							.weapon-durability-bar{
								flex: 1;
								height: 18px;
								display: flex;
								flex-direction: row;
								align-items: center;
								background-color: rgba(200, 200, 200, 1);
								border: 1px solid rgba(180, 180, 180, 1);
								border-radius: 3px;
								overflow: hidden;
								.weapon-durability-item{
									height: 100%;
									.weapon-durability-value{
										text-align: center;
										color: white;
										font-size: 12px;
										text-shadow: -1px 0 rgba(200, 200, 200, 1),  /* 左边 */
										             0 1px rgba(200, 200, 200, 1),  /* 下边 */  
										             1px 0 rgba(200, 200, 200, 1),  /* 右边 */  
										             0 -1px rgba(200, 200, 200, 1); /* 上边 */  
									}
								}
							}
						}
					}
					.weapon-durability-box-bottom{
						width: 100%;
						margin-top: 2px;
						.weapon-durability-group{
							width: 100%;
							display: flex;
							flex-direction: row;
							align-items: center;
							.weapon-durability-level{
								margin-right: 5px;
							}
							.weapon-durability-group-bar{
								width: 100%;
								height: 18px;
								display: flex;
								flex-direction: row;
								align-items: center;
								background-color: rgba(200, 200, 200, 1);
								border: 1px solid rgba(180, 180, 180, 1);
								border-radius: 3px;
								overflow: hidden;
								margin: 1px 0;
								.weapon-durability-item{
									height: 100%;
									.weapon-durability-value{
										text-align: center;
										color: white;
										font-size: 12px;
										text-shadow: -1px 0 rgba(200, 200, 200, 1),  /* 左边 */
										             0 1px rgba(200, 200, 200, 1),  /* 下边 */  
										             1px 0 rgba(200, 200, 200, 1),  /* 右边 */  
										             0 -1px rgba(200, 200, 200, 1); /* 上边 */  
									}
								}
							}
						}
					}
				}
				.weapon-durability-show{
					max-height: 300px;
				}
				.weapon-ammo{
					width: 100%;
					display: flex;
					flex-direction: row;
					flex-wrap: wrap;
					justify-content: left;
					box-sizing: border-box;
					.weapon-ammo-item{
						display: flex;
						justify-content: space-between;
						flex: 0 0 calc(25%);
						box-sizing: border-box;
						align-items: center;
						border: 1px solid rgba(223, 223, 223, 1);
						margin-top: 1px;
						border-radius: 3px;
						.weapon-ammo-icon{
							width: 26px;
							height: 26px;
							background-color: rgba(45, 45, 45, 1);
							border-radius: 3px;
							box-sizing: border-box;
							display: flex;
							justify-content: center;
							align-items: center;
							flex: 1;
							image{
								width: 24px;
								height: 24px;
							}
						}
						.weapon-ammo-number{
							text-align: center;
							padding: 1px;
							flex: 1;
						}
					}
				}
				.weapon-coatings{
					.weapon-coatings-value-box{
						display: flex;
						flex-direction: column;
						.weapon-coatings-value{
							display: flex;
							flex-direction: row;
							align-items: center;
							.weapon-coatings-image{
								width: 26px;
								height: 26px;
								display: flex;
								align-items: center;
								justify-content: center;
								image{
									width: 24px;
									height: 24px;
								}
							}
						}
					}
				}
			}
		}
	}
</style>
