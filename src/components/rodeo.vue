<template>
<div>
    <div v-if="!dIsLoged">
      <login @loadUserData="fLoadUserData"></login>
    </div>
    <div v-else>
      <venues :pUser="this.dUser" :pruebo="'hola'"></venues>
    </div>
  </div>
</template>

<script>
  import Login from '../components/login.vue';
  import Venues from '../components/venues.vue';

  export default {
    name: 'Rodeo',
    components: {
      Login,
      Venues
    },
    data() {
      return {
        dUser: null,
        dIsLoged: false,
      }
    },
    mounted(){
    },
    methods:{
      fLoadUserData(value){
        this.dUser = value
        this.dIsLoged = true
      }
    }
  }  

</script>

