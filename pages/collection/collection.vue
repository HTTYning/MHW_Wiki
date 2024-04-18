<template>
	<view>
		<view class="collection-content">
			<view class="collection-title">
				<view>My Collection</view>
			</view>
			<view class="collection-tabbar">
				<view class="tabbar-item" @click="switchBar('AllCollections')">
					All Collections
				</view>
				<view class="tabbar-item" @click="switchBar('FilterCollection')">
					Filter Collection
					<view class="tabbar-item-icon" :class="filterBoxStatus?'tabbar-item-icon-active':''">
						<image src="../../static/collection/show.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="tabbar-animation-bar" :class="FilterActive?'tabbar-animation-bar-switch':''"></view>
			</view>
			<view class="filter-tab-Masking" @click="hiddenFilterBox()" :class="filterBoxStatus?'filter-tab-Masking-active':''">
				<view class="filter-tab-box" @click.stop="">
					<view class="filter-tab-title">
						Filter Collection
					</view>
					<view class="filter-tab-hidden-btn" @click="hiddenFilterBox()">
						<image src="../../static/collection/hidden.png" mode="aspectFit"></image>
					</view>
					<view class="filter-tab-list">
						<view class="filter-tab-item" @click="choiceFilterType('weapon')">Weapon</view>
						<view class="filter-tab-item" @click="choiceFilterType('item')">Item</view>
					</view>
				</view>
			</view>
			<view class="collection-list">
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				FilterActive:false,
				filterBoxStatus:false,
				filterType:''
			};
		},
		onLoad() {
			
		},
		methods:{
			switchBar(barName){
				// 切换到筛选按钮时让筛选状态变为true，并把窗口状态变为true打开窗口
				if(barName == 'AllCollections'){
					this.FilterActive = false;
					return;
				}
				if(barName == 'FilterCollection'){
					this.FilterActive = true;
					this.filterBoxStatus = true;
					return;
				}
			},
			hiddenFilterBox(){
				// 判断选择的筛选类型是否为空，如果为空则是没选类型，让选项卡回到所有收藏并关闭窗口
				// 如果不是为空则是选择过类型，只需要关闭窗口，保持筛选类型
				if(this.filterType != ''){
					this.filterBoxStatus = false;
				}else{
					this.filterBoxStatus = false;
					this.FilterActive = false;
				}
			},
			choiceFilterType(opFilterType){
				// 选择类型事件，选择类型后关闭窗口，并保存好选择的类型
				this.filterBoxStatus = false;
				this.filterType = opFilterType;
				console.log(this.filterType);
			}
		}
	}
</script>

<style lang="scss">
	.collection-content{
		display: flex;
		flex-direction: column;
		align-items: center;
		box-sizing: border-box;
		.collection-title{
			width: 100%;
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
			background-color: #f7f7f7;
			border-bottom: 2px solid #dfdfdf;
			padding: 10px 0;
			color: #515151;
			font-size: 18px;
			font-weight: bold;
			box-sizing: border-box;
		}
		.collection-tabbar{
			width: 100%;
			height: 40px;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			align-items: center;
			box-sizing: border-box;
			position: relative;
			.tabbar-item{
				height: 40px;
				display: flex;
				flex-direction: row;
				justify-content: center;
				align-items: center;
				color: #515151;
				font-weight: bold;
				font-size: 14px;
				flex: 0 0 calc(50%);
				box-sizing: border-box;
				z-index: 1;
				.tabbar-item-icon{
					display: flex;
					flex-direction: row;
					align-items: center;
					justify-content: center;
					margin-left: 6px;
					transform: rotate(0deg);
					transition: all 0.2s ease-out 0s;
					image{
						width: 20px;
						height: 20px;
					}
				}
				.tabbar-item-icon-active{
					transform: rotate(180deg);
				}
			}
			.tabbar-animation-bar{
				width: 50%;
				height: 38px;
				position: absolute;
				top: 0;
				transform: translateX(0%);
				background-color: #f7f7f7;
				border-bottom: 2px solid #dfdfdf;
				z-index: -1;
				transition: all 0.2s ease-out 0s;
			}
			.tabbar-animation-bar-switch{
				transform: translateX(100%);
			}
		}
		.filter-tab-Masking{
			width: 100%;
			height: 100%;
			background-color: rgba(0, 0, 0, 0.6);
			position: fixed;
			z-index: 2;
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
			transition: all 0.2s ease-out 0s;
			top: -100%;
			opacity: 0;
			.filter-tab-box{
				width: 70%;
				height: 70%;
				background-color: rgba(255, 255, 255, 1);
				border-radius: 10px;
				border: 3px solid #dfdfdf;
				position: relative;
				.filter-tab-title{
					width: 100%;
					height: 20px;
					font-size: 20px;
					font-weight: bold;
					display: flex;
					flex-direction: row;
					justify-content: center;
					align-items: center;
					padding: 10px 0;
					border-bottom: 2px solid #dfdfdf;
					color: rgba(45, 45, 45, 1);
				}
				.filter-tab-hidden-btn{
					top: 10px;
					right: 10px;
					position: absolute;
					image{
						width: 22px;
						height: 22px;
					}
				}
				.filter-tab-list{
					display: flex;
					flex-direction: column;
					align-items: center;
					box-sizing: border-box;
					.filter-tab-item{
						width: 100%;
						height: 40px;
						display: flex;
						flex-direction: row;
						justify-content: center;
						align-items: center;
						box-sizing: border-box;
						border: 1px solid #dfdfdf;
						color: #515151;
					}
				}
			}
		}
		.filter-tab-Masking-active{
			top: 0%;
			opacity: 1;
		}
	}
</style>
