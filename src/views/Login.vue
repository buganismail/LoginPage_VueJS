<template>
	 <title>Login Page</title>
<div class="login">

				<form>
					<label for="chk" aria-hidden="true">Login</label>
					<input v-model="userData.username" type="text" placeholder="Username" class="input mb-3" />
                   <input v-model="userData.password" type="password" placeholder="Password" class="input mb-3" />
				   
                    <button @click="onSubmit" class="default-button">Login</button>
                     <span class="text-center mt-3 text-sm">
                     
                    <router-link :to="{ name : 'RegisterPage'}" class="text-red-900 hover:text-black">
                    SIGN UP
                   </router-link>
                    </span>
				</form>
			</div>
</template>

<script>
	import CryptoJS from "crypto-js"
	export default {
	  data() {
		return {
		  userData : {
			username : null,
			password : null
		  }
		};
		},
		methods : {
			onSubmit() {
			   const password = CryptoJS.HmacSHA1(this.userData.password, this.$store.getters._saltKey).toString();
			   this.$appAxios
			   .get(`/users?username=${this.userData.username}&password=${password}`)
			   .then(login_response => {
				if (login_response?.data?.length > 0) {
				  console.log(login_response);
				this.$store.commit("setUser", login_response?.data[0]);
				this.$router.push({ name : "HomePage"});
				} else {
				  alert("Böyle bir kullanıcı bulunamadı ...");
				}
			   })
			   .catch(e => console.log(e));
			}
		}
	  };
	
	</script>


