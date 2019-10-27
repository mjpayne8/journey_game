<template lang="html">
  <div>
      <ul v-for="country in countryList">
        <li @click="handleClick(country)">
          <img :src="country.flag" alt="flag">
          <p>{{country.name}}</p>
        </li>
      </ul>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
  data(){
    return {
      selectedCountry: {}
    }
  },
  props: ["countryList"],
  methods: {
    handleClick(country){
      this.selectedCountry = country;
      eventBus.$emit("selected-country", this.selectedCountry)
      }
  }
}
</script>

<style lang="css" scoped>
div {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
div img{
  height: 80px;
  width: 160px;
}
li {
  list-style: none;
}
</style>
