<template lang="html">
<div>
  <img :src="info.flag" alt="flag">
  <ul>
    <li>Country: {{info.name}}</li>
    <li>Population: {{info.population}}</li>
    <li>Region: {{info.region}}</li>
  </ul>
</div>


</template>

<script>


export default {
  props: ["info"]
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
</style>
