<template>
  <div class="Make Reservation">
    <input placeholder="kasutaja nimi" v-model="username">
    <input placeholder="parool" v-model="password">
    <button v-on:click="login"> Sisesta</button>
  </div>
</template>

<script>
export default {
  name: "Login",
  data: function () {
    return {
      username: "",
      password: ""
    }
  },
  methods: {
    login: function () {
      let request = {
        userName: this.username,
        password: this.password
      }
      this.$http.post("/user/login", request)
          .then(response => {
            alert("Tere tulemast " + response.data.firstName + " " + response.data.lastName)
            console.log(response.data)
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    }
  }

}
</script>

<style scoped>

</style>