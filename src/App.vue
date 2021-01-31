<template>
  <div id="app">
    <GetPersons/>
  </div>
</template>

<script>
import GetPersons from "@/components/Persons/GetPersons";
export default {
  name: 'App',
  components: {
    GetPersons
  }
}
</script>

<style>
#app {
}
</style>
