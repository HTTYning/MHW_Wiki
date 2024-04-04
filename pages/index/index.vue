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
					{{item.name}}
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
						img:''
					},
					{
						name:'long-sword',
						img:''
					},
					{
						name:'sword-and-shield',
						img:''
					},
					{
						name:'dual-blades',
						img:''
					},
					{
						name:'hammer',
						img:''
					},
					{
						name:'hunting-horn',
						img:''
					},
					{
						name:'lance',
						img:''
					},
					{
						name:'gunlance',
						img:''
					},
					{
						name:'switch-axe',
						img:''
					},
					{
						name:'charge-blade',
						img:''
					},
					{
						name:'insect-glaive',
						img:''
					},
					{
						name:'light-bowgun',
						img:''
					},
					{
						name:'heavy-bowgun',
						img:''
					},
					{
						name:'bow',
						img:''
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
			transition: all 0.3s ease-out 0s;
			max-height: 1000px;
			.weapon-item{
				padding: 14px 20px;
				display: flex;
				justify-content: left;
				flex: 0 0 calc(50%);
				border: 1px solid rgba(223, 223, 223, 1);
				background-color: white;
				box-sizing: border-box;
			}
		}
		.weapon-flod-list-dis{
			max-height: 0;
			opacity: 0;
			visibility: hidden;
		}
	}
</style>
