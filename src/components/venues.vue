<template>
  <div>
    <div v-if="dSelectedVenue == null">
      <v-container>
        <v-row class="d-flex justify-center mt-6">
          <v-col v-for="item in dVenues" :key="item.id" cols="12" sm="6" md="3">
            <v-card class="mx-auto" max-width="344" outlined>
              <v-card-text>
                <div>{{item.name}}</div>
                <p class="text--primary">
                  <b>Address: </b>{{item.address}}
                </p>
                <p class="text--primary">
                  <b>Phone Number: </b>{{item.phoneNumber}}
                </p>
                <p class="text--primary">
                  <b>url: </b>{{item.url}}
                </p>
                <p class="text--primary">
                  <b>Opening Time: </b>{{item.openingTime}}
                </p>
                <p class="text--primary">
                  <b>closingTime: </b>{{item.closingTime}}
                </p>
              </v-card-text>

              <v-card-actions>
                <v-btn outlined rounded text @click="fViewOffer(item)">
                  View Offers
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <div v-if="dSelectedVenue !== null && dSelectedOffer == null">
      <v-btn outlined rounded text @click="dSelectedVenue=null">Back</v-btn>
        <v-container>
          <v-row class="d-flex justify-center mt-6">
            <v-col v-for="item in dOffers" :key="item.id" cols="12" sm="6" md="3">
              <v-card class="mx-auto" max-width="344" outlined>
                <v-card-text>
                  <div>{{item.title}}</div>
                  <p class="text--primary">
                    <b>price: </b>{{item.price}} €
                  </p>
                  <p class="text--primary">
                    <b>detail: </b>{{item.detail}}
                  </p>
                </v-card-text>
                <v-card-actions>
                  <v-btn outlined rounded text @click="fViewOfferDetail(item)">
                    View Detail
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
    </div>
    <div v-if="dSelectedVenue !== null && dSelectedOffer !== null">
      <v-btn outlined rounded text @click="dOffer=null, dSelectedOffer=null">Back</v-btn>
        <v-container>
          <v-row class="d-flex justify-center mt-6">
            <v-col cols="12" sm="6" md="3">
              <v-card class="mx-auto" max-width="344" outlined>
                <v-card-text>Hola
                  <div>{{dOffer.title}}</div>
                  <p class="text--primary">
                    <b>category: </b>{{dOffer.categoryName}} / {{dOffer.subcategoryName}} 
                  </p>
                  <p class="text--primary">
                    <b>detail: </b>{{dSelectedOffer.detail}}
                  </p>
                  <p class="text--primary">
                    <b>price: </b>{{dSelectedOffer.price}} {{dOffer.currency}}
                  </p>
                  <p class="text--primary">
                    <b>Starts: </b>{{dOffer.selectedStart}}
                  </p>
                  <p class="text--primary">
                    <b>price: </b>{{dOffer.selectedEnd}}
                  </p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
    </div>
  </div>
</template>

<script>
import axios from 'axios'    

export default ({
  props: {
    pUser: {
      type: Object,
      required: true
    }
  }, 
  data() {
    return {
      dVenues: null,
      dSelectedVenue: null,
      dSelectedOffer: null ,
      dOffers: null,
      dOffer: null
    }
  },
  mounted(){
    this.fgetVenues()
  },
  methods:{
    fViewOffer(item){
      this.dSelectedVenue = item
      this.fgetOffers()
    },
    fViewOfferDetail(item){
      this.dSelectedOffer = item
      this.fgetOfferDetails()
    },
    //Services
    fgetVenues(){
      axios.get("https://api.rodeoworld.co.uk/venues/list?idBusiness="+this.pUser.id, { 'headers': { 'Authorization': this.pUser.token }}).then(response =>{
        this.dVenues = response.data
      }).catch(er =>{
        console.log(er)
      })  
    },
    fgetOffers(){
      axios.get("https://api.rodeoworld.co.uk/offers/list/"+this.dSelectedVenue.id, { 'headers': { 'Authorization': this.pUser.token }}).then(response =>{
        this.dOffers = response.data
      }).catch(er =>{
        console.log(er)
      })  
    },
    fgetOfferDetails(){
      axios.get("https://api.rodeoworld.co.uk/offers/"+this.dSelectedOffer.id, { 'headers': { 'Authorization': this.pUser.token }}).then(response =>{
        this.dOffer = response.data
      }).catch(er =>{
        console.log(er)
      })  
    }
  }
})
</script>
