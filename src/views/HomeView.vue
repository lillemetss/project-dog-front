<template>
  <div class="home">

    <h1>MILJON MUKIT MTÜ</h1>
    <img alt="Cocker spaniel" src="https://www.pdsa.org.uk/media/8262/cocker-spaniel-looking-at-camera-min.jpg" class="img-fluid">
    <HelloWorld msg="Tule meie koertega jalutama"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}


</script>
