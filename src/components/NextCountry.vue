<template lang="html">
  <div>
      <li v-for="country in countryList">
        <img :src="country.flag" alt="flag">
        <p>{{country.name}}</p>
      </li>
  </div>
</template>

<script>
export default {
  props: ["countryList"]
}
</script>

<style lang="css" scoped>
div {
  display: flex;
  flex-wrap: wrap;
}
div img{
  height: 80px;
  width: 160px;
}
</style>
