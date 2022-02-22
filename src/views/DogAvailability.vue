<template>
  <div class="DogAvailability">
    <h1> Koerte saadavus </h1>
    <input placeholder="Vali kuupäev" v-model="requiredDate">
    <input placeholder="Algus kellaaeg" v-model="requiredStartTime">
    <input placeholder="Lõpu kellaaeg" v-model="requiredEndTime">
    <button v-on:click="dogAvailability"> Sisesta</button>
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

    }
  },
  methods: {
    dogAvailability: function () {
      let request = {
        requiredDate: this.requiredDate,
        requiredStartTime: this.requiredStartTime,
        requiredEndTime: this.requiredEndTime
      }
      this.$http.post("/time/date", request)
          .then(response => {
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