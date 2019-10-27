<template>
  <div id="app">
    <div v-if='!Object.keys(currentCountry).length'>
      <h1>Welcome to the journey game</h1>
      <p>The aim is to get from your starting point to your end point passing through neighbouring countries</p>
      <button type="button" @click='initialiseCountries()'>Start</button>
    </div>
    <div v-if='Object.keys(currentCountry).length'>
      <current-info :info = "currentCountry" ></current-info>
    <div>
    <div v-if='Object.keys(currentCountry).length'>
        <p>Where to next?</p>
        <next-country :country-list = "findCountries(currentCountry.borders)"></next-country>
    </div>
  </div>
</template>

<script>
import CurrentInfo from './components/CurrentInfo.vue'
import NextCountry from './components/NextCountry.vue'

export default {
  name: 'app',
  data() {
    return {
      allCountries: [],
      endCountry: {},
      currentCountry: {},
    }
  },
  mounted(){
    fetch('https://restcountries.eu/rest/v2/all')
    .then(response => response.json())
    .then(data => this.allCountries = data);
  },
  components: {
    "current-info": CurrentInfo,
    "next-country": NextCountry
  },
  computed: {
    stepsNumber: function(){
      if(this.allCountries.includes(this.endCountry)){
      return this.getSteps(this.currentCountry,this.endCountry)
      }
    }
  },
  methods: {
    getRandom: function(){
      const indexOfStart = Math.floor(this.allCountries.length*Math.random());
      const indexOfEnd = Math.floor(this.allCountries.length*Math.random());
      this.currentCountry = this.allCountries[indexOfStart];
      this.endCountry = this.allCountries[indexOfEnd];
    },
    findCountries: function(countriesToFind){
      return this.allCountries.filter(country => countriesToFind.includes(country.alpha3Code))
    },
    getSteps: function(countryA,countryB){
      let hasFoundCountry = false;
      let steps = 0;
      let travelCountries = [countryA];
      let countriesCanTravel = [];
      while (!hasFoundCountry){
        steps += 1;
        travelCountries.forEach(country => countriesCanTravel = countriesCanTravel.concat(country.borders))
        hasFoundCountry = countriesCanTravel.includes(countryB.alpha3Code);
        travelCountries = this.findCountries(countriesCanTravel);
        }
      return steps;
    },
     // initialiseCountries: function(){
     //  const indexOfStart = Math.floor(this.allCountries.length*Math.random());
     //  this.currentCountry = this.allCountries[indexOfStart];
     //  let stepCountry = [this.currentCountry];
     //  let possibleCountries =[];
     //  let steps = 0;
     //  while(steps<3){
     //    steps += 1;
     //    // console.log(steps);
     //    // console.log(stepCountry);
     //   if (!stepCountry[0]){
     //      // console.log('in if1');
     //      const indexOfStart = Math.floor(this.allCountries.length*Math.random());
     //      this.currentCountry = this.allCountries[indexOfStart];
     //      stepCountry = [this.currentCountry];
     //      steps = 0;
     //      }
     //    const randomNumber =  Math.floor(stepCountry[0].borders.length*Math.random());
     //    console.log(randomNumber);
     //    stepCountry = this.findCountries([stepCountry[0].borders[randomNumber]]);
     //    }
     //    this.endCountry = stepCountry[0];
     //  }
    initialiseCountries: function(){
        const chosenCountries = this.findCountries(['FRA','POL'])
        this.currentCountry = chosenCountries[0];
        this.endCountry = chosenCountries[1]
    }
  }
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
</style>
