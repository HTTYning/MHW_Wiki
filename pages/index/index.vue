<template>
	<view class="content">
		<view class="weapon-fold">
			<view class="weapon-fold-title">
				<view>武器</view>
				<view class="weapon-fold-btn" @click="switchWeapon()">
					⬇
				</view>
			</view>
			<view class="weapon-fold-list" :class="weaponStatus?'':'weapon-flod-list-dis'">
				<view class="weapon-item" @click="goWeaponPage(item.name)" v-for="(item,index) in weaponType" :key="index">
					<view class="weapon-icon">
						<image :src="item.img" mode="aspectFit"></image>
					</view>
					<view class="weapon-name">
						{{item.name}}
					</view>
				</view>
			</view>
		</view>
		<view class="">
			123
		</view>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				weaponType:[
					{
						name:'great-sword',
						img:'/static/index-weapon/weapon_type_great-sword.png'
					},
					{
						name:'long-sword',
						img:'/static/index-weapon/weapon_type_long-sword.png'
					},
					{
						name:'sword-and-shield',
						img:'/static/index-weapon/weapon_type_sword-and-shield.png'
					},
					{
						name:'dual-blades',
						img:'/static/index-weapon/weapon_type_dual-blades.png'
					},
					{
						name:'hammer',
						img:'/static/index-weapon/weapon_type_hammer.png'
					},
					{
						name:'hunting-horn',
						img:'/static/index-weapon/weapon_type_hunting-horn.png'
					},
					{
						name:'lance',
						img:'/static/index-weapon/weapon_type_lance.png'
					},
					{
						name:'gunlance',
						img:'/static/index-weapon/weapon_type_gunlance.png'
					},
					{
						name:'switch-axe',
						img:'/static/index-weapon/weapon_type_switch-axe.png'
					},
					{
						name:'charge-blade',
						img:'/static/index-weapon/weapon_type_charge-blade.png'
					},
					{
						name:'insect-glaive',
						img:'/static/index-weapon/weapon_type_insect-glaive.png'
					},
					{
						name:'light-bowgun',
						img:'/static/index-weapon/weapon_type_light-bowgun.png'
					},
					{
						name:'heavy-bowgun',
						img:'/static/index-weapon/weapon_type_heavy-bowgun.png'
					},
					{
						name:'bow',
						img:'/static/index-weapon/weapon_type_bow.png'
					}
				],
				weaponStatus:true
			}
		},
		onLoad() {
			
		},
		methods: {
			switchWeapon(){
				this.weaponStatus = !this.weaponStatus;
			},
			goWeaponPage(weaponName){
				uni.navigateTo({
					url: '/pages/weapons/weapons?&weaponName='+weaponName
				});
			}
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #f8f9fa;
	}
	.weapon-fold{
		width: 95%;
		height: auto;
		margin: 0 auto;
		border: 1px solid rgba(223, 223, 223, 1);
		background-color: rgba(247, 247, 247, 1);
		border-radius: 3px;
		overflow: hidden;
		.weapon-fold-title{
			padding: 8px 0;
			font-size: 20px;
			font-weight: bold;
			text-align: center;
			border: 1px solid rgba(223, 223, 223, 1);
			position: relative;
			z-index: 1000;
			.weapon-fold-btn{
				position: absolute;
				top: 50%;
				transform: translateY(-50%);
				right: 20px;
			}
		}
		.weapon-fold-list{
			display: flex;
			flex-direction: row;
			flex-wrap: wrap;
			transition: all 0.5s ease-out 0s;
			max-height: 1000px;
			.weapon-item{
				padding: 14px 20px;
				display: flex;
				justify-content: left;
				flex: 0 0 calc(50%);
				border: 1px solid rgba(223, 223, 223, 1);
				background-color: white;
				box-sizing: border-box;
				align-items: center;
				.weapon-icon{
					width: 24px;
					height: 24px;
					margin-right: 10px;
					image{
						width: 100%;
						height: 100%;
					}
				}
				.weapon-name{
					font-weight: bold;
					color: rgba(45, 45, 45, 1);
				}
			}
		}
		.weapon-flod-list-dis{
			max-height: 0;
			opacity: 0.3;
			visibility: hidden;
		}
	}
</style>
