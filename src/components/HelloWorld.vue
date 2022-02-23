<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>

      <a href="https://www.doggiedrawings.net/dow" target="_blank" rel="noopener">Joonistused maailma erinevatest koertest</a>
    </p>
    <p>Miljon Mukit MTÜ, registrikood 80006666 </p>
    <p>Uus 12, Tallinn, 10111 </p>
    <p>Meiega saad kontakti läbi muki@miljon.mukit</p>
    <a href="https://goo.gl/maps/tW68aPZGmBAdvg7eA" target="blank" rel="noopener">Leia meid kaardilt</a>
    <ul>
    </ul>

<!--    <h3>Ecosystem</h3>-->
<!--    <ul>-->
<!--      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>-->
<!--      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>-->
<!--      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>-->
<!--      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>-->
<!--      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>-->
<!--    </ul>-->
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #660000;
}
</style>
