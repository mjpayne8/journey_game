<template lang="html">
<div>
<div>
  <h1>Current Location</h1>
  <img :src="info.flag" alt="flag">
  <ul>
    <li>Country: {{info.name}}</li>
    <li>Population: {{info.population}}</li>
    <li>Region: {{info.region}}</li>
  </ul>
</div>
<div>
  <h1>Destination</h1>
  <img :src="destination.flag" alt="flag">
  <ul>
    <li>Country: {{destination.name}}</li>
    <li>Population: {{destination.population}}</li>
    <li>Region: {{destination.region}}</li>
  </ul>
</div>
</div>
</template>

<script>


export default {
  props: ["info","destination"]
}
</script>

<style lang="css" scoped>
  div {
    display: flex;
    flex: center;
  }
  div img{
    height: 80px;
    width: 160px;
  }
  h1 {
    display: inline-block;
  }
</style>
