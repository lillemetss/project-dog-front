<template>
  <div class="home">
    <h1> MILJON MUKIT MTÜ </h1>
    <img alt="Dog" src="https://hips.hearstapps.com/ghk.h-cdn.co/assets/17/30/pembroke-welsh-corgi.jpg">
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  data: function (){
    return{
    username: "",
      password:""
    }
  },
  methods:{

    savaDataToSessionStorage: function (){
      sessionStorage.setItem('optionalUser', this.username)
      sessionStorage.setItem('optionalPassword', this.password)
      sessionStorage.setItem('optionalUser', JSON.stringify(this.username))
      sessionStorage.setItem('optionalPassword', JSON.stringify(this.password))
    }
  },
  components: {
    HelloWorld


    //   methods: {
    //
    // beforeMount: function (){
    //   this.username= sessionStorage.getItem('optionalUser')
    //   this.options= JSON.parse(sessionStorage.getItem('optionalUser'))
    // },
    // savaDataToSessionStorage: function (){
    //   sessionStorage.setItem('optionalUser', this.username)
    //   sessionStorage.setItem('optionalUser', JSON.stringify(this.username))
    // }
    // }
  }
}

</script>
