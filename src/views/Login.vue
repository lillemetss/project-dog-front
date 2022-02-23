<template>
  <div class="Login">

    <h3>Logi sisse:</h3>
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
      password: "",
      inputFirstName: "",
      inputUserId: ""
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
            this.inputFirstName = response.data.firstName
            this.inputUserId = response.data.userId
            this.getNameAndRedirectToReserveDogPage()
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    },

    getNameAndRedirectToReserveDogPage: function(){
      this.$router.push({name: 'ReserveDog',
        query: {firstNameParam: this.inputFirstName, userIdParam: this.inputUserId}});
    }

  },

  mounted() {

  },

}
</script>

<style scoped>

</style>