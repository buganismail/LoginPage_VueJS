<template>
    <div class="signup">
				<form>
					<label for="chk" aria-hidden="true">Sign up</label>
					<input v-model="userData.fullname" type="text" placeholder="Full Name" class="input mb-3" />
                   <input v-model="userData.username" type="text" placeholder="Username" class="input mb-3" />
                   <input v-model="userData.password" type="password" placeholder="Password" class="input mb-3" />
                  <button class="default-button" @click="onSave">Sign Up</button>
                  <span class="text-center mt-3 text-sm">
                   <router-link :to="{ name : 'LoginPage'}" class="text-red-900 hover:text-black">
                  LOGIN
                 </router-link>
                  </span>
				</form>
			</div>
</template>

<script>
  import CryptoJs from "crypto-js"
  export default {
    data() {
      return {
        userData : {
          username : null,
          fullname : null,
          password : null
        }
      }
    },
    methods : {
      onSave(){
        const password = CryptoJs.HmacSHA1(this.userData.password, this.$store.getters._saltKey).toString();
        this.$appAxios.post("/users", { ...this.userData, password}).then(registered_user_response =>{
          console.log('registered_user_response :>> ', registered_user_response);
          this.$router.push({ name : "LoginPage" });
        });
        // const decryptedPassword = CryptoJs.AES.decrypt(cryptedPassword, key).toString(CryptoJS.enc.Utf8);
  
        // console.log(this.userData);
      }
    }
  }; 
  </script>