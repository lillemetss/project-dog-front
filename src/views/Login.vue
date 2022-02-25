<template>
  <div class="Login">
    <h3> Logi sisse: </h3>
    <input placeholder="kasutaja nimi" v-model="username">
    <input type=password  placeholder="parool" v-model="password">
    <button v-on:click="login"> Sisesta</button>
<!--    <div>-->
<!--      <select v-model="selected">-->
<!--      <option disabled value=" ">Vali konto</option>-->
<!--        <option v-for="option in options" : value="option.userId">{{option.userName}}</option>-->
<!--      </select>-->
<!--      <br>-->
<!--      <span>Valitud konto<{{selected}}/span>-->
<!--    </div>-->

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
      inputUserId: "",
    }
  },
  // beforeMount (){
  //   this.username= sessionStorage.getItem('optionalUser')
  //   this.options= JSON.parse(sessionStorage.getItem('optionalUser'))
  // },
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
            this.savaDataToSessionStorage()
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    },

    getNameAndRedirectToReserveDogPage: function(){
      this.$router.push({name: 'ReserveDog',
        query: {firstNameParam: this.inputFirstName, userIdParam: this.inputUserId}});
    },
    savaDataToSessionStorage: function (){
      sessionStorage.setItem('optionalUser', this.username)
      sessionStorage.setItem('optionalPassword', this.password)
    }
  },

  mounted() {

  },

}
</script>

<style scoped>

</style>