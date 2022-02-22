<template>
  <div id="app">
    <nav>
      <router-link to="/">Kodu</router-link>
      |
      <router-link to="/login">Logi sisse</router-link>
      |
      <router-link to="/bronn">Broneeri aeg</router-link>
      |
      <router-link to="/about">Meist</router-link>
    </nav>
    <router-view/>

<!--    <div>-->
<!--      <CalendarWidget/>-->
<!--    </div>-->
  </div>
</template>


<!--<script>-->
<!--import CalendarWidget from "@/components/CalendarWidget";-->

<!--export default {-->
<!--  name: 'App',-->
<!--  components: {-->
<!--    CalendarWidget-->
<!--  }-->
<!--}-->
<!--</script>-->
<!--<style scoped>-->
<!--.container {-->
<!--  max-width: 300px;-->
<!--}-->

<!--</style>-->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
<script>
import CalendarWidget from "@/components/CalendarWidget";

export default {
  components: {CalendarWidget}
}
</script>