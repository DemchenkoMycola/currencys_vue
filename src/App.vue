<template>
  <b-container id="app">
    <b-row class="table-header text-center">
      <b-col>Logo</b-col>
      <b-col>Full name</b-col>
      <b-col>Name</b-col>
      <b-col>Price</b-col>
      <b-col>SUPLY</b-col>
    </b-row>
    <b-row class="table_item" v-for="i in currencys" >
      <b-col>
        <img :src='`https://www.cryptocompare.com${i.CoinInfo.ImageUrl}`'>
      </b-col>
      <b-col>
        <p>{{i.CoinInfo.FullName}}</p>
      </b-col>
      <b-col>
        <p>{{i.CoinInfo.Name}}</p>
      </b-col>
      <b-col>
        <p>{{i.RAW.USD.PRICE}}</p>
      </b-col>
      <b-col>
        <p>{{i.RAW.USD.SUPPLY}}</p>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>

import axios from "axios";

export default {
  name: 'app',
  components: {

  },
  data(){
    return {
      currencys: null,
    };
  },
  methods: {
    getCurrencys(){
      axios
        .get('https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD')
        .then(response => {
          this.currencys = response.data.Data;
          // sort
          this.currencys.sort(function(a,b){
            return b.RAW.USD.PRICE - a.RAW.USD.PRICE
          })

        });
    },
  },
  mounted(){
    this.getCurrencys();
  },
  created() {
    this.interval = setInterval(() => this.getCurrencys(), 30000);
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


.table-header{
  background: #f2f2f2;
  padding: 20px;
}

.table_item{

}

.table_item div {
  padding: 5px;
  border: 1px solid #f0f0f0;
}

img{
  width: 50px;
  height: 50px;
}
</style>
