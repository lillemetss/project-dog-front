<template>
  <div class="ReserveDog">
    <div v-if="displayMainView">
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="logOut">Logi välja</button>
      <br>
      <br>
      <h3>Tere {{ firstName }}!</h3>
      <img alt="Doggo" src="https://www.pdsa.org.uk/media/7707/beagle-page-image-2-min.jpg" class="img-fluid">
      <br>
      <br>
      <button type="button" class="btn btn-secondary" v-on:click="displayDogsSearchDiv">Broneeri uus jalutuskäik
      </button>
      <br>
      <br>
      <button type="button" class="btn btn-secondary" v-on:click="getAllUserReservations">Minu jalutuskäigud
      </button>
    </div>

    <div v-if="displayDogSearch">

      <br>
      <h3> Siin saad broneerida uue aja koeraga jalutamiseks: </h3>
      <input type=date min="2022-02-28" placeholder="Vali kuupäev" v-model="requiredDate">
      <input placeholder="Algus kellaaeg" v-model="requiredStartTime">
      <input placeholder="Lõpu kellaaeg" v-model="requiredEndTime">
      <br>
      <br>
      <button type="button" class="btn btn-secondary" v-on:click="dogAvailability">Otsi vabu koeri</button>
    </div>

    <div v-if="displayDogSearchResults">

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
          <td>{{ row.dogName }}</td>
          <td>{{ row.dogDescription }}</td>
          <td>
            <button type="button" class="btn btn-success" v-on:click="selectDog(row)">vali mind</button>
          </td>

        </tr>
        </tbody>
      </table>
    </div>

    <div v-if="displaySelectedDogData">

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

      <button type="button" class="btn btn-success" v-on:click="reserveDog">Kinnita jalutuskäigu broneering</button>
      <br>
      <br>
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="displayMainViewDiv">Tagasi...</button>


    </div>

    <div v-if="displayReservationConfirmation">


      <h2>Broneeringu kinnitus </h2>
      <br>
      <br>
      Hea {{firstName}}!
      <br>
      <br>
      {{ dog.dogName }} ootab sind jalutama {{requiredDate}} kell {{requiredStartTime}}. Head jalutamist!
      <br>
      <br>
      {{reservationNumber}}
      <br>
      <br>
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="displayMainViewDiv">Tagasi pealehele...</button>
      <br>
      <br>
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="logOut">Logi välja</button>

    </div>

    <div v-if="displayAllUsersReservations">

      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="displayMainViewDiv">Tagasi...</button>
      <br>
      <br>
      <table class="table table-hover">
        <thead>
        <tr>
          <th scope="col">Koera nimi</th>
          <th scope="col">Koera ID</th>
          <th scope="col">Jalutuskäigu kuupäev</th>
          <th scope="col">Algus</th>
          <th scope="col">Lõpp</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in reservations">
          <td>{{ row.dogName }}</td>
          <td>{{ row.dogId }}</td>
          <td>{{ row.date }}</td>
          <td>{{ row.startTime }}</td>
          <td>{{ row.endTime }}</td>

        </tr>
        </tbody>
      </table>
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
      displayDogSearch: false,
      displayDogSearchResults: false,
      displaySelectedDogData: false,
      displayReservationConfirmation: false,
      displayAllUsersReservations: false,
      reservations: {},
      date: "",
      startTime: "",
      endTime: ""

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
            this.displayDogSearch = false
            this.displayDogSearchResults = true
            console.log(response.data)
          })
          .catch(error => {
            alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
            console.log(error)
          })
    },

    selectDog: function (dog) {
      this.dog = dog
      this.displayDogSearchResults = false
      this.displaySelectedDogData = true
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
        this.displaySelectedDogData = false
        this.displayReservationConfirmation = true
        this.dogs = response.data
        console.log(response.data)
      }).catch(error => {
        alert(error.response.data.message)
        console.log(error)
      })

    },
    logOut() {
      sessionStorage.clear()
      this.$router.push({name:'Login'})
    },

    displayMainViewDiv: function () {
      this.displayMainView = true
      this.displaySelectedDogData = false
      this.displayAllUsersReservations = false
      this.displayReservationConfirmation = false
    },

    displayDogsSearchDiv: function () {
      this.hideAllDivs()
      this.displayDogSearch = true
    },

    hideAllDivs: function () {
      this.displayMainView = false

    },


    getAllUserReservations: function () {
      this.$http.get("/reserve/history", {
            params: {
              userId: this.userId
            }
          }
      ).then(response => {
        this.reservations = response.data
        this.hideAllDivs()
        this.displayAllUsersReservations = false
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    }
  }

}
</script>

<style scoped>

</style>