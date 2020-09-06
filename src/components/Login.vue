<template>
	<form action="/sonething" id="app" @submit="ActionPost">
			<div class="login__item">
				<div class="login__text">Логин</div>
				<input
					type="text" 
					name="login" 
					id="login" 
					v-model.trim="login"
					:class="{log__error:error.login}"
				>
			</div>
			<p v-if="error.login">{{error.login}}</p>
			<div class="login__item">
				<div class="login__text">Телефон</div>
				<input 
				type="text" 
				name="phone" 
				id="phone"
				v-model.trim="phone"
				:class="{log__error:error.phone}"
				>
			</div>
			<p v-if="error.phone">{{error.phone}}</p>
			<div class="login__item">
				<div class="login__text">Пароль</div>
				<input 
				type="password" 
				name="password" 
				id="password"
				v-model.trim="password"
				:class="{log__error:error.password}"
				>
			</div>
			<p v-if="error.password">{{error.password}}</p>
			<input type="submit" value="Войти">
		</form>
</template>

<script>

const loginCheck = /^[a-zA-Z0-9_]{5,}$/;
const phoneCheck = /^[0-9]{11}$/;

export default{

	data(){
		return{
			login: '',
			phone: '',
			password: '',
			error: [],
			isPressed: false
		};
	},

	computed: {
		isLoginValid() {
			return loginCheck.test(this.login);
		},
		isPhoneValid() {
			return phoneCheck.test(this.phone);
		},
		isPasswordValid() {
			return (this.password.length >= 6)?true:false;
		},
	},

	watch:{
		login(){
			// this.login = value;
			this.validateLogin();
		},
		phone(){
			// this.phone = value;
			this.validatePhone();
		},
		password(){
			// this.password = value;
			this.validatePassword();
		}
	},
	methods: {

		validateLogin(){
			if (this.isPressed){
				if (loginCheck.test(this.login)) this.error['login'] = ''
				else if (!this.login) this.error['login'] = 'Обязательное поле'
				else if (this.login.length < 5) this.error['login'] = 'Логин должен состоять минимум из 5 символов'
				else this.error['login'] = 'Неверный формат логина (только английские буквы, числа и нижнее подчёркивание)';
			}
		},
		validatePhone(){
			if (this.isPressed){
				if (phoneCheck.test(this.phone)) this.error['phone'] = ''
				else if (!this.phone) this.error['phone'] = 'Обязательное поле'
				else this.error['phone'] = 'Неверный формат телефона (11 цифр)';
			}
		},
		validatePassword(){
			if (this.isPressed){
				if (this.password.length >= 6) this.error['password'] = ''
				else if (!this.password) this.error['password'] = 'Обязательное поле'
				else this.error['password'] = 'Неверный формат пароля (пароль должен быть минимум 6 символов)';
			}
		},

		ActionPost: function(e) {
			this.isPressed = true;
			if(!this.isLoginValid || !this.isPhoneValid || !this.isPasswordValid) {
				e.preventDefault();
				if (!this.isLoginValid) this.validateLogin();
				if (!this.isPhoneValid) this.validatePhone();
				if (!this.isPasswordValid) this.validatePassword();
				this.$forceUpdate();
			}
			else alert("Форма отправлена!");
		}
	}
}
</script>>

<style>

form{
	margin: 0 auto;
	width: 200px;
}

.login__text{
	font-size: 16px;
	font-weight: bold;
	padding-bottom: 10px;
}

input[type=text], input[type=password] {
	width: 100%;
	height: 30px;
	padding-left: 10px;
	margin-bottom: 10px;
	text-decoration: none;
	outline: none;
	border: 1px solid rgb(204, 204, 204);
	border-radius: 3px;
}

input[type=text]:focus{
	border-color: rgb(54, 139, 250);
	outline: none;
	/* text-decoration: none; */
}

input[type=submit]{
	width: 60px;
	height: 30px;
	cursor: pointer;
	background-color: #fff;
	text-decoration: none;
	border: 1px solid rgb(204, 204, 204);
	border-radius: 3px;
	color: rgb(139, 139, 139);
	transition: background linear .3s;
}

input[type=submit]:hover{
	background-color: rgb(54, 139, 250);
	color: #fff;
	border: none;
}

input[type=submit]:focus {
	text-decoration: none;
	border-color: rgb(54, 139, 250);
}

p {
	padding-top: 0px;
	margin-top: 0px;
	font-size: 12px; color:red;
}

.log__error {
	border: 1px solid red !important;
}
</style>>