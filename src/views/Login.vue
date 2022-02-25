<template>
  <div class="Login">

    <h3>Logi sisse:</h3>
    <input placeholder="Kasutajanimi" v-model="username">
    <input type=password placeholder="Parool" v-model="password">
    <button type="button" class="btn btn-outline-dark" v-on:click="login"> Sisesta</button>

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

  beforeMount(){
    if (sessionStorage.getItem('userId') !== null) {
      this.$router.push({
        name: 'ReserveDog',
        query: {firstNameParam: sessionStorage.getItem('firstName'), userIdParam: sessionStorage.getItem('userId')}
      });
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
            this.saveDataToSessionStorage()
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    },

    saveDataToSessionStorage: function (){
      sessionStorage.setItem('userName', this.username)
      sessionStorage.setItem('password', this.password)
      sessionStorage.setItem('firstName', this.inputFirstName)
      sessionStorage.setItem('userId', this.inputUserId)
    },


    getNameAndRedirectToReserveDogPage: function () {
      this.$router.push({
        name: 'ReserveDog',
        query: {firstNameParam: sessionStorage.getItem('firstName'), userIdParam: sessionStorage.getItem('userId')}
      });
    }

  },

}
</script>


<style scoped>

</style>