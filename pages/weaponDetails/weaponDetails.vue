<template>
	<view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				weaponId:null,
				weaponObj:{}
			};
		},
		onLoad(option){
			this.weaponId = option.weaponId;
			console.log(this.weaponId);
			this.getWeaponDetails();
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
				} catch(err) {
					console.log("请求失败! " + err)
				}
			},
		}
	}
</script>

<style lang="scss">

</style>
