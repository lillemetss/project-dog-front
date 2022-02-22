<template>
  <div class="DogAvailability">
    <h1> Broneeri aeg! </h1>
    <input placeholder="Vali kuupäev" v-model="requiredDate">
    <input placeholder="Algus kellaaeg" v-model="requiredStartTime">
    <input placeholder="Lõpu kellaaeg" v-model="requiredEndTime">
    <button v-on:click="dogAvailability">Otsi vabu koeri</button>

    <table>
      <tr>
        <th>Koera nimi</th>
        <th>Kirjeldus</th>
        <th></th>
      </tr>
      <tr v-for="row in dogs">
        <td>{{ row.dogName }}</td>
        <td>{{ row.dogDescription }}</td>
        <td><button>vali mind</button></td>

      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "DogAvailability",
  data: function () {
    return {
      requiredDate: "yyyy-mm-dd",
      requiredStartTime:0 ,
      requiredEndTime: 0,
      dogs:{}
    }
  },
  methods: {
    dogAvailability: function () {
      let request = {
        requiredDate: this.requiredDate,
        requiredStartTime:  this.requiredStartTime,
        requiredEndTime: this.requiredEndTime
      }
      this.$http.post("/time/date", request)
          .then(response => {
            this.dogs= response.data
            alert("Edukalt borneeritud koer " + response.data.dogName + " " + response.data.dogDescription)
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