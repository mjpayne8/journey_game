<template>
  <div id="app">
    <div v-if='!Object.keys(currentCountry).length'>
      <h1>Welcome to the journey game</h1>
      <p>The aim is to get from your starting point to your end point passing through neighbouring countries</p>
      <button type="button" @click='initialiseCountries()'>Start</button>
    </div>
    <p>{{endCountry}}</p>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

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

  },
  computed: {
    stepsNumber: function(){
      if(this.allCountries.includes(this.endCountry)){
      this.getSteps(this.currentCountry,this.endCountry)
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
      return this.allCountries.filter(country => countriesToFind.includes(country.alpha3code))
    },
    getSteps: function(countryA,countryB){
      let hasFoundCountry = false;
      let steps = 0;
      const countriesCanTravel = [];
      const travelCountries = [countryA]
      while (!hasFoundCountry){
        steps += 1;
        // console.log(steps);
        travelCountries.forEach(country => countriesCanTravel.concat(country.alpha3code));
        hasFoundCountry = travelCountries.includes(countryB.alpha3code);
        }
      return steps;
    },
    initialiseCountries: function(){
      const indexOfStart = Math.floor(this.allCountries.length*Math.random());
      this.currentCountry = this.allCountries[indexOfStart];
      let stepCountry = [this.currentCountry];
      let possibleCountries =[];
      let steps = 0;
      while(steps<20){
        steps += 1;
        console.log(steps);
        console.log(stepCountry);
        if (!stepCountry[0]){
          console.log('in if1');
          const indexOfStart = Math.floor(this.allCountries.length*Math.random());
          this.currentCountry = this.allCountries[indexOfStart];
          stepCountry = [this.currentCountry];
          steps = 0;
          }
        const randomNumber =  Math.floor(stepCountry[0].borders.length*Math.random());
        console.log(randomNumber);
        stepCountry = this.findCountries([stepCountry[0].borders[randomNumber]]);
        }
        this.endCountry = stepCountry[0];
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
