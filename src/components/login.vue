<template>
    <div>
        <v-form>
        <v-container>
          <v-row class="d-flex justify-center mt-6">
            <v-col cols="12" sm="6" md="3">
              <v-text-field v-model="dEmail" label="Email"></v-text-field>
              <v-text-field v-model="dPassword" :type="'password'" label="Password"></v-text-field>
              <v-btn depressed color="primary" @click="fDoLogin">Login</v-btn>
              <div v-if="dLoginError">
                <strong class="red--text text--lighten-1">Wrong mail or password</strong>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </div>
</template>

<script>
import axios from 'axios'    

export default ({
    data() {
      return {
        dEmail: "",
        dPassword: "",
        dLoginError: false
      }
    },
    mounted(){
        console.log('entro login')
    },
    methods:{
      fDoLogin(){
        let payload = {
          email: this.dEmail, //support@rodeoworld.co.uk
          password: this.dPassword //1q2w3e4r
        }
        axios.post("https://api.rodeoworld.co.uk/businesses/login", payload).then(response =>{
          console.log('response', response)
          this.dLoginError = false
          this.$emit("loadUserData",response.data)
        }).catch(er =>{
          console.log(er)
          this.dLoginError = true
        })  
      }
    }
})
</script>
