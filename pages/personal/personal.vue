<template>
	<view>
		<view class="personal-info">
			<view class="info-box-left">
				<view class="info-headImage">
					<image src="../../static/personal/defaultHeadImage.png" mode="aspectFit"></image>
				</view>
			</view>
			<view class="info-box-middle">
				<view class="info-name">
					丛雨
				</view>
				<view class="info-introduction">
					什么都没留下
				</view>
			</view>
			<view class="info-box-right" @click="windowSwitch()">
				登出
			</view>
		</view>
		<view class="setting-box" @click="goSettingPage()">
			<view class="btn-text">Setting</view>
			<view class="btn-icon">
				<image src="../../static/personal/right.png" mode="aspectFit"></image>
			</view>
		</view>
		<view class="window-bg" :class="windowStatus ? 'window-bg-show':''" @click="windowSwitch()">
			<view class="window-box" @click.stop="">
				<view class="window-title" v-if="accountOperation == 'login'">
					Login
				</view>
				<view class="window-title" v-if="accountOperation == 'register'">
					Register
				</view>
				<view class="window-hidden-btn" @click="windowSwitch()">
					<image src="../../static/personal/hidden.png" mode="aspectFit"></image>
				</view>
				<view class="ipt-list login-ipt-list" v-if="accountOperation == 'login'">
					<view class="ipt-item" @click="switchLoginIptStatus('Phone')" v-if="isEmailLogin == false">
						<view class="item-text" :class="loginIptStatus.Phone ? 'item-text-show':''">
							<view>Phone</view>
						</view>
						<view class="item-ipt-box" :class="loginIptStatus.Phone ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="loginIptValue.Phone" :focus="loginIptStatus.Phone" @blur="blurLoginIpt('Phone')" type="number" maxlength="13"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchLoginIptStatus('Email')" v-if="isEmailLogin == true">
						<view class="item-text" :class="loginIptStatus.Email ? 'item-text-show':''">
							<view>Email</view>
						</view>
						<view class="item-ipt-box" :class="loginIptStatus.Email ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="loginIptValue.Email" :focus="loginIptStatus.Email" @blur="blurLoginIpt('Email')" type="text" maxlength="30"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchLoginIptStatus('Password')">
						<view class="item-text" :class="loginIptStatus.Password ? 'item-text-show':''">
							<view>Password</view>
						</view>
						<view class="item-ipt-box" :class="loginIptStatus.Password ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="loginIptValue.Password" :focus="loginIptStatus.Password" @blur="blurLoginIpt('Password')" password type="text" maxlength="30"/>
						</view>
					</view>
				</view>
				<view class="ipt-list register-ipt-list" v-if="accountOperation == 'register'">
					<view class="ipt-item" @click="switchRegisterIptStatus('Nickname')">
						<view class="item-text" :class="registerIptStatus.Nickname ? 'item-text-show':''">
							<view>Nickname</view>
							<view class="tip-text">
								{{registerTip.Nickname}}
							</view>
						</view>
						<view class="item-ipt-box" :class="registerIptStatus.Nickname ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="registerIptValue.Nickname" :focus="registerIptStatus.Nickname" @input="checkIptValue('Nickname')" @blur="blurRegisterIpt('Nickname')" type="text" maxlength="20"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchRegisterIptStatus('Phone')">
						<view class="item-text" :class="registerIptStatus.Phone ? 'item-text-show':''">
							<view>Phone</view>
							<view class="tip-text">
								{{registerTip.Phone}}
							</view>
						</view>
						<view class="item-ipt-box" :class="registerIptStatus.Phone ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="registerIptValue.Phone" :focus="registerIptStatus.Phone" @input="checkIptValue('Phone')" @blur="blurRegisterIpt('Phone')" type="number" maxlength="13"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchRegisterIptStatus('Email')">
						<view class="item-text" :class="registerIptStatus.Email ? 'item-text-show':''">
							<view>Email (Can be empty)</view>
							<view class="tip-text">
								{{registerTip.Email}}
							</view>
						</view>
						<view class="item-ipt-box" :class="registerIptStatus.Email ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="registerIptValue.Email" :focus="registerIptStatus.Email" @input="checkIptValue('Email')" @blur="blurRegisterIpt('Email')" type="text" maxlength="30"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchRegisterIptStatus('Password')">
						<view class="item-text" :class="registerIptStatus.Password ? 'item-text-show':''">
							<view>Password</view>
							<view class="tip-text">
								{{registerTip.Password}}
							</view>
						</view>
						<view class="item-ipt-box" :class="registerIptStatus.Password ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="registerIptValue.Password" :focus="registerIptStatus.Password" @input="checkIptValue('Password')" @blur="blurRegisterIpt('Password')" password type="text" maxlength="30"/>
						</view>
					</view>
					<view class="ipt-item" @click="switchRegisterIptStatus('ConfirmPassword')">
						<view class="item-text" :class="registerIptStatus.ConfirmPassword ? 'item-text-show':''">
							<view>ConfirmPassword</view>
							<view class="tip-text">
								{{registerTip.ConfirmPassword}}
							</view>
						</view>
						<view class="item-ipt-box" :class="registerIptStatus.ConfirmPassword ? 'item-ipt-box-show':''">
							<input class="item-ipt" v-model="registerIptValue.ConfirmPassword" :focus="registerIptStatus.ConfirmPassword" @input="checkIptValue('ConfirmPassword')" @blur="blurRegisterIpt('ConfirmPassword')" password type="text" maxlength="30"/>
						</view>
					</view>
				</view>
				<view class="operation-btn-box">
					<view class="operation-btn-item login-btn" v-if="accountOperation == 'login'">
						<button class="operation-btn" type="default">Login</button>
					</view>
					<view class="operation-btn-item register-btn" v-if="accountOperation == 'register'">
						<button class="operation-btn" type="default">Register</button>
					</view>
					<view class="operation-btn-item switch-login-btn" v-if="accountOperation == 'register'" @click="switchOperation('login')">
						<button class="operation-btn" type="default">Go Login</button>
					</view>
					<view class="operation-btn-item switch-register-btn" v-if="accountOperation == 'login'" @click="switchOperation('register')">
						<button class="operation-btn" type="default">Go Register</button>
					</view>
					<view class="operation-btn-item email-login-btn" v-if="accountOperation == 'login'&&isEmailLogin == false" @click="switchLoginMethod()">
						<button class="operation-btn" type="default">Email Login (If any)</button>
					</view>
					<view class="operation-btn-item email-login-btn" v-if="accountOperation == 'login'&&isEmailLogin == true" @click="switchLoginMethod()">
						<button class="operation-btn" type="default">Phone Login</button>
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
				windowStatus:false,
				accountOperation:'login',
				isEmailLogin:false,
				loginIptStatus:{
					Phone:false,
					Password:false,
					Email:false
				},
				loginIptValue:{
					Phone:'',
					Password:'',
					Email:''
				},
				registerIptStatus:{
					Nickname:false,
					Phone:false,
					Password:false,
					ConfirmPassword:false,
					Email:false
				},
				registerIptValue:{
					Nickname:'',
					Phone:'',
					Password:'',
					ConfirmPassword:'',
					Email:''
				},
				registerTip:{
					Nickname:'',
					Phone:'',
					Email:'',
					Password:'',
					ConfirmPassword:''
				},
				iptValidate:{
					Nickname:false,
					Phone:false,
					Password:false,
					ConfirmPassword:false,
					Email:false
				}
			};
		},
		onLoad(){
			this.windowStatus = true;
		},
		methods:{
			windowSwitch(){
				this.windowStatus = !this.windowStatus;
			},
			switchLoginIptStatus(key){
				switch (key){
					case "Nickname":
						this.loginIptStatus.Nickname = true;
						break;
					case "Phone":
						this.loginIptStatus.Phone = true;
						break;
					case "Password":
						this.loginIptStatus.Password = true;
						break;
					case "ConfirmPassword":
						this.loginIptStatus.ConfirmPassword = true;
						break;
					case "Email":
						this.loginIptStatus.Email = true;
						break;
					default:
						return;
						break;
				}
			},
			blurLoginIpt(key){
				switch (key){
					case "Nickname":
						if(this.loginIptValue.Nickname == ''){
							this.loginIptStatus.Nickname = false;
						}else{
							return;
						}
						break;
					case "Phone":
						if(this.loginIptValue.Phone == ''){
							this.loginIptStatus.Phone = false;
						}else{
							return;
						}
						break;
					case "Password":
						if(this.loginIptValue.Password == ''){
							this.loginIptStatus.Password = false;
						}else{
							return;
						}
						break;
					case "ConfirmPassword":
						if(this.loginIptValue.ConfirmPassword == ''){
							this.loginIptStatus.ConfirmPassword = false;
						}else{
							return;
						}
						break;
					case "Email":
						if(this.loginIptValue.Email == ''){
							this.loginIptStatus.Email = false;
						}else{
							return;
						}
						break;
					default:
						return;
						break;
				}
			},
			switchLoginMethod(){
				this.isEmailLogin = !this.isEmailLogin;
			},
			switchRegisterIptStatus(key){
				switch (key){
					case "Nickname":
						this.registerIptStatus.Nickname = true;
						break;
					case "Phone":
						this.registerIptStatus.Phone = true;
						break;
					case "Password":
						this.registerIptStatus.Password = true;
						break;
					case "ConfirmPassword":
						this.registerIptStatus.ConfirmPassword = true;
						break;
					case "Email":
						this.registerIptStatus.Email = true;
						break;
					default:
						return;
						break;
				}
			},
			blurRegisterIpt(key){
				switch (key){
					case "Nickname":
						if(this.registerIptValue.Nickname == ''){
							this.registerIptStatus.Nickname = false;
						}else{
							return;
						}
						break;
					case "Phone":
						if(this.registerIptValue.Phone == ''){
							this.registerIptStatus.Phone = false;
						}else{
							return;
						}
						break;
					case "Password":
						if(this.registerIptValue.Password == ''){
							this.registerIptStatus.Password = false;
						}else{
							return;
						}
						break;
					case "ConfirmPassword":
						if(this.registerIptValue.ConfirmPassword == ''){
							this.registerIptStatus.ConfirmPassword = false;
						}else{
							return;
						}
						break;
					case "Email":
						if(this.registerIptValue.Email == ''){
							this.registerIptStatus.Email = false;
						}else{
							return;
						}
						break;
					default:
						return;
						break;
				}
			},
			switchOperation(op){
				this.accountOperation = op;
			},
			checkIptValue(key){
				switch (key){
					case "Nickname":
						if(this.registerIptValue.Nickname != ''){
							this.registerTip.Nickname = 'This nickname is already in use';
							this.iptValidate.Nickname = false;
						}else{
							this.registerTip.Nickname = 'Cannot be empty';
						}
						break;
					case "Phone":
						if(this.registerIptValue.Phone != ''){
							if(this.phoneValidate(this.registerIptValue.Phone)){
								this.registerTip.Phone = '';
								this.iptValidate.Phone = true;
							}else{
								this.registerTip.Phone = 'Please enter the correct formate';
								this.iptValidate.Phone = false;
							}
						}else{
							this.registerTip.Phone = 'Cannot be empty';
						}
						break;
					case "Email":
						if(this.registerIptValue.Email != ''){
							let emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
							if(emailRegex.test(this.registerIptValue.Email)){
								this.registerTip.Email = '';
								this.iptValidate.Email = true;
							}else{
								this.registerTip.Email = 'Please enter the correct email address';
								this.iptValidate.Email = false;
							}
						}else{
							this.registerTip.Email = 'Cannot be empty';
						}
						break;
					case "Password":
						if(this.registerIptValue.Password != ''){
							if(this.registerIptValue.Password.length >= 6){
								this.registerTip.Password = '';
								this.iptValidate.Password = true;
							}else{
								this.registerTip.Password = 'Please enter a password with more than 6 digits';
								this.iptValidate.Password = false;
							}
						}else{
							this.registerTip.Password = 'Cannot be empty';
							this.iptValidate.Password = false;
						}
						break;
					case "ConfirmPassword":
						if(this.registerIptValue.ConfirmPassword != ''){
							if(this.registerIptValue.Password === this.registerIptValue.ConfirmPassword){
								this.registerTip.ConfirmPassword = '';
								this.iptValidate.ConfirmPassword = true;
							}else{
								this.registerTip.ConfirmPassword = 'Confirm that the password is different from the password';
								this.iptValidate.ConfirmPassword = false;
							}
						}else{
							this.registerTip.ConfirmPassword = 'Cannot be empty';
							this.iptValidate.ConfirmPassword = false;
						}
						break;
					default:
						return;
						break;
				}
			},
			phoneValidate(phone){
				let validateArr = [];
				let chinaPhoneRegex = /^1[3-9]\d{9}$/;
				if(chinaPhoneRegex.test(phone)){
					validateArr.push(true);
				}else{
					validateArr.push(false);
				}
				let usPhoneRegex = /^\(?([0-9]{3})\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})$/;
				if(usPhoneRegex.test(phone)){
					validateArr.push(true);
				}else{
					validateArr.push(false);
				}
				let ukPhoneRegex = /^\(?(07[45789]\d{2}|00447[45789]\d{2})\)?\s?\d{3}\s?\d{3}$/;
				if(ukPhoneRegex.test(phone)){
					validateArr.push(true);
				}else{
					validateArr.push(false);
				}
				let indiaPhoneRegex = /^(\+?91)?[7-9]\d{9}$/;
				if(indiaPhoneRegex.test(phone)){
					validateArr.push(true);
				}else{
					validateArr.push(false);
				}
				let internationalPhoneRegex = /^\+(?:[0-9] ?){6,14}[0-9]$/;
				if(internationalPhoneRegex.test(phone)){
					validateArr.push(true);
				}else{
					validateArr.push(false);
				}
				for (var i = 0; i < validateArr.length; i++) {
					if(validateArr[i]){
						console.log(validateArr[i]);
						return true;
					}
				}
				return false;
			},
			goSettingPage(){
				uni.navigateTo({
					url: '/pages/setting/setting'
				});
			},
		}
	}
</script>

<style lang="scss">
	.window-bg{
		width: 100%;
		height: 100%;
		position: fixed;
		left: 0;
		top: -50%;
		transform: translateY(-50%);
		background-color: rgba(0, 0, 0, 0.6);
		display: flex;
		justify-content: center;
		align-items: center;
		transition: all 0.2s ease-out 0s;
		opacity: 0;
		.window-box{
			width: 90%;
			height: 93%;
			border-radius: 10px;
			background-color: rgba(255, 255, 255, 1);
			border: 3px solid #dfdfdf;
			position: relative;
			display: flex;
			flex-direction: column;
			.window-hidden-btn{
				top: 10px;
				right: 10px;
				position: absolute;
				image{
					width: 22px;
					height: 22px;
				}
			}
			.window-title{
				width: 100%;
				height: 20px;
				font-size: 20px;
				font-weight: bold;
				display: flex;
				flex-direction: row;
				justify-content: center;
				align-items: center;
				padding: 10px 0;
				border-bottom: 2px solid #515151;
				color: rgba(45, 45, 45, 1);
			}
			.ipt-list{
				padding: 8px 14px;
				.ipt-item{
					width: 100%;
					border: 1px solid #dfdfdf;
					margin-bottom: 3px;
					border-radius: 5px;
					padding: 6px 10px;
					display: flex;
					flex-direction: column;
					justify-content: center;
					box-sizing: border-box;
					.item-text{
						width: 100%;
						font-size: 18px;
						transition: all 0.2s ease-out 0s;
						color: black;
						display: flex;
						flex-direction: row;
						justify-content: space-between;
						.tip-text{
							color: red;
							font-size: 12px;
						}
					}
					.item-text-show{
						font-size: 14px;
						color: rgba(80, 80, 80, 1);
					}
					.item-ipt-box{
						width: 100%;
						max-height: 0;
						transition: all 0.2s ease-out 0s;
						overflow: hidden;
						.item-ipt{
							margin-top: 3px;
						}
					}
					.item-ipt-box-show{
						max-height: 200px;
					}
				}
			}
			.operation-btn-box{
				width: 100%;
				display: flex;
				flex-direction: row;
				flex-wrap: wrap;
				padding: 0 14px;
				box-sizing: border-box;
				justify-content: center;
				.operation-btn-item{
					min-width: 50%;
					margin-bottom: 3px;
					padding: 0 2px;
					box-sizing: border-box;
					.operation-btn{
						font-size: 16px;
						border: 1px solid #dfdfdf;
					}
				}
			}
		}
	}
	.window-bg-show{
		top: 50%;
		opacity: 1;
	}
	.personal-info {
		width: 100%;
		height: 120px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		padding: 5px 10px;
		box-sizing: border-box;
		border-bottom: 2px solid #bfbfbf;
		.info-box-left{
			flex-shrink: 0;
			.info-headImage{
				width: 100px;
				height: 100px;
				border-radius: 100%;
				display: flex;
				justify-content: center;
				align-items: center;
				overflow: hidden;
				image{
					width: 100px;
					height: 100px;
				}
			}
		}
		.info-box-middle{
			height: 100%;
			flex: 1;
			display: flex;
			flex-direction: column;
			justify-content: flex-start;
			padding: 14px 0;
			box-sizing: border-box;
			margin-left: 10px;
			.info-name{
				font-size: 20px;
				font-weight: bold;
				color: black;
			}
			.info-introduction{
				color: #2d2d2d;
				margin-top: 5px;
				font-size: 14px;
			}
		}
		.info-box-right{
			padding: 5px 10px;
			border-radius: 7px;
			flex-shrink: 0;
			background-color: #bfbfbf;
			color: white;
			font-weight: bold;
			margin-right: 20px;
		}
	}
	.setting-box{
		display: flex;
		flex-direction: row;
		align-content: center;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 10px;
		border-bottom: 2px solid #bfbfbf;
		.btn-text{
			color: #2d2d2d;
			font-weight: bold;
		}
		.btn-icon{
			image{
				width: 20px;
				height: 20px;
			}
		}
	}
</style>
