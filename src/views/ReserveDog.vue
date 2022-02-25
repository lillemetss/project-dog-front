<template>
  <div class="ReserveDog">
    <div v-if="displayMainView">
      <h3>Tere, {{ this.firstName }}!</h3>
      <h3> Broneeri aeg: </h3>
      <input type=date min="2022-02-25" placeholder="Vali kuupäev" v-model="requiredDate">
      <input placeholder="Algus kellaaeg" v-model="requiredStartTime">
      <input placeholder="Lõpu kellaaeg" v-model="requiredEndTime">
      <button v-on:click="dogAvailability">Otsi vabu koeri</button>

      <table v-if="dogs.length > 0">
        <tr>
          <th>Koera nimi</th>
          <th>Kirjeldus</th>
          <th></th>
        </tr>
        <tr v-for="row in dogs">
          <td>{{ row.dogName }}</td>
          <td>{{ row.dogDescription }}</td>
          <td>
            <button v-on:click="selectDog(row)">vali mind</button>
          </td>

        </tr>
      </table>
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
      <br>
      <div v-if="displayDogAvailability">
      <button v-on:click="displayMainViewDiv">Tagasi</button>
    </div>
    </div>
    <div v-if="displayReservationConfirmation">
      Hea {{firstName}}!
      <br>
     {{dog.dogName}} ootab Teid jalutama {{requiredDate}} kell {{requiredStartTime}}. Head jalutust!
      <br>
      Broneerigu number: {{reservationNumber}}.
    </div>

  </div>
</template>

<script>
export default {
  name: "ReserveDog",
  data: function () {
    return {
      requiredDate: "",
      requiredStartTime: "",
      requiredEndTime: "",
      dogs: {},
      dog: {},
      user: {},
      displayMainView: true,
      displayDogAvailability: false,
      displayReservationConfirmation: false,
      dogName: "",
      dogId: "",
      reservationNumber: "",
      userId: this.$route.query.userIdParam,
      firstName: this.$route.query.firstNameParam

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
        alert(" Koer edukalt broneeritud! " +response.data.reservationNumber)
        this.reservationNumber=response.data.reservationNumber
        this.hideAllDivs()
        this.displayReservationConfirmation = true
        console.log(response.data)
      }).catch(error => {
        alert(error.response.data.message + " Error code: " + error.response.data.errorCode)
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

    }
  }

}
</script>

<style scoped>

</style>