<template>
  <div class="ReserveDog">
    <div v-if="displayMainView">

      <button v-on:click="logOut">Logi välja</button>

      <div class="ReservationSearch">

        <br>
        <br>
        <img alt="Doggo" src="https://www.pdsa.org.uk/media/7707/beagle-page-image-2-min.jpg" class="img-fluid">
        <br>
        <br>
        <h3>Tere, {{ this.firstName }}!</h3>
        <h3> Siit saad broneerida aja koeraga jalutamiseks: </h3>
        <input type=date min="2022-02-25" placeholder="Vali kuupäev" v-model="requiredDate">
        <input placeholder="Algus kellaaeg" v-model="requiredStartTime">
        <input placeholder="Lõpu kellaaeg" v-model="requiredEndTime">
        <button v-on:click="dogAvailability">Otsi vabu koeri</button>
        <br>
        <br>
        <table class="table table-hover" v-if="dogs.length > 0">
          <thead>
          <tr>
            <th scope="col">Koera nimi</th>
            <th scope="col">Kirjeldus</th>
            <th scope="col">Broneeri</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="row in dogs">
            <th scope="row"></th>
            <td>{{ row.dogName }}</td>
            <td>{{ row.dogDescription }}</td>
            <td>
              <button v-on:click="selectDog(row)">vali mind</button>
            </td>

          </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div v-if="displayDogAvailability">

      Koera ID <input disabled v-model="dog.dogId">
      <br>
      <br>
      Koera nimi <input disabled v-model="dog.dogName">
      <br>
      <br>
      Jalutuskäigu kuupäev <input disabled v-model="requiredDate">
      <br>
      <br>
      Jalutuskäik algab <input disabled v-model="requiredStartTime">
      <br>
      <br>
      Jalutuskäik lõpeb <input disabled v-model="requiredEndTime">
      <br>
      <br>
      Konto ID <input disabled v-model="userId">
      <br>
      <br>

      <button v-on:click="reserveDog">Kinnita jalutuskäigu broneering</button>

    </div>

    <br>
    <br>

    <div v-if="displayDogAvailability">
      <button v-on:click="displayMainViewDiv">Tagasi...</button>
    </div>

    <div v-if="displayReservationConfirmation">


      <h2>Broneeringu kinnitus </h2>
      <br>
      <br>
      Hea {{ firstName }}!
      <br>
      <br>
      {{ dog.dogName }} ootab sind jalutama {{ requiredDate }} kell {{ requiredStartTime }}. Head jalutamist!
      <br>
      <br>
      {{ reservationNumber }}
      <br>
      <br>
      <button v-on:click="logOut">Logi välja</button>

    </div>

    <div>
    </div>

  </div>
</template>

<script>
export default {
  name: "ReserveDog",
  data: function () {
    return {
      requiredDate: "yyyy-mm-dd",
      requiredStartTime: "",
      requiredEndTime: "",
      dogs: {},
      dog: {},
      user: {},
      dogName: "",
      dogId: "",
      reservationNumber: "",
      userId: sessionStorage.getItem('userId'),
      firstName: sessionStorage.getItem('firstName'),
      displayMainView: true,
      displayDogAvailability: false,
      displayReservationConfirmation: false,
      displayAllUserReservations: false

    }
  },
  methods: {
    dogAvailability: function () {
      let request = {
        requiredDate: this.requiredDate,
        requiredStartTime: this.requiredStartTime,
        requiredEndTime: this.requiredEndTime,
      }
      this.$http.post("/time/date", request)
          .then(response => {
            this.dogs = response.data
            this.displayMainViewDiv()
            console.log(response.data)
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    },

    selectDog: function (dog) {
      this.hideAllDivs()
      this.dog = dog
      this.displayDogAvailability = true
    },

    reserveDog: function () {
      let request = {
        requiredDate: this.requiredDate,
        requiredStartTime: this.requiredStartTime,
        requiredEndTime: this.requiredEndTime,
        dogId: this.dog.dogId,
        userId: this.userId
      }
      this.$http.post("/reserve/dog", request
      ).then(response => {
        alert("Koer jalutamiseks broneeritud." + response.data.reservationNumber)
        this.reservationNumber = response.data.reservationNumber
        this.hideAllDivs()
        this.displayReservationConfirmation = true
        this.dogs = response.data
        console.log(response.data)
      }).catch(error => {
        alert(error.response.data.message)
        console.log(error)
      })

    },

    displayMainViewDiv: function () {
      this.displayMainView = true
      this.displayDogAvailability = false
    },

    hideAllDivs: function () {
      this.displayMainView = false
      this.displayDogAvailability = false

    },

    logOut: function () {
      sessionStorage.clear()
      this.$router.push({name: 'Login'})
    },

  },

  displayAllUserReservations: function () {
    this.$http.get("/reserve/history", {
          params: {
            userId: this.userId
          }
        }
    ).then(response => {
      this.reservations = response.data
      this.hideAllDivs()
      console.log(response.data)
    }).catch(error => {
      console.log(error)
    })
  }


}
</script>

<style scoped>

</style>