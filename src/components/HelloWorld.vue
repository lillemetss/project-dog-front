<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>

    <footer>
    <p>Miljon Mukit MTÜ, registrikood 80006666</p>
    <p>Uus 12, Tallinn, 10111</p>
    <p> Meiega saad kontakti läbi muki@miljon.mukit</p>
    <p><a href="https://goo.gl/maps/tW68aPZGmBAdvg7eA" target="blank" rel="noopener">Leia meid kaardilt</a>
    </p>
  </footer>
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
