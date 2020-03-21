<template>
  <div id="app">
    <TrelloPop />
  </div>
</template>

<script>

import TrelloPop from './components/TrelloPop.vue';

export default {
  name: 'App',
  components: {
    TrelloPop
  }, 
 data(){
   return {
     token: ' ',
     boards : []
   }
 },
  mounted() {
    this.getEmployees()
  },
 methods:{
   async getEmployees(){
      try {
        const API_ME_BASE = "https://api.trello.com/1/members/me/";
        const KEY = "47836fa3a77b0663415e2e5f7152222b";
        const TOKEN = this.token;
        const response = await fetch(`${API_ME_BASE}boards?key=${KEY}&token=${TOKEN}&fields=name`);
        const data = await response.json()
        console.log(data)
      } catch (error) {
        console.log("error")
       console.error(error)
    }
   },
 }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
