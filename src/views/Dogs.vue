<template>
  <div class="Dogs">
    <br>
    <div v-if="displayMainView">

      <h4>Soovime meie koertele parimat elu</h4>
      <h4>Tutvu meie koertaga
        <button type="button" class="btn btn-secondary" v-on:click="getAllDogs">siin</button>
      </h4>
      <br>
      <img alt="Doggo"
           src="https://images.unsplash.com/photo-1537123547273-e59f4f437f1b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8YmljaG9ufGVufDB8fDB8fA%3D%3D&w=1000&q=80"
           class="img-fluid">


    </div>
    <div v-if="displayDogsInfoView">
      <h4></h4>
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="displayMainViewDiv">Tagasi...</button>
      <br>

      <table class="table table-hover">
        <thead>
        <tr>
          <th scope="col">Koera nimi</th>
          <th scope="col">Koera lühiiseloomustus</th>
          <th scope="col">Vaata lähemalt</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in dogs">
          <td>{{ row.dogName }}</td>
          <td>{{ row.dogDescription }}</td>
          <button type="button" class="btn btn-success" v-on:click="selectDog(row)">Minust</button>

        </tr>
        </tbody>
      </table>

    </div>
    <div v-if="displaySelectedDogData">

      <h3>{{dog.dogName}}</h3>

      <table class="table table-hover">
        <thead>
        <tr>
          <th scope="col">Iseloomulikud jooned</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in characteristics">
          <td>{{ row.characteristicTag }}</td>
        </tr>
        </tbody>
      </table>



    </div>

  </div>
</template>

<script>
export default {
  name: "Dogs.vue",
  data: function () {
    return {
      dogId: "",
      dogName: "",
      dogDescription: "",
      dogs: {},
      dog: {},
      characteristics: {},
      characteristicTag: "",
      displayMainView: true,
      displayDogsInfoView: false,
      displaySelectedDogData: false

    }
  },
  methods: {
    getDogsInfo: function () {
      this.$http.get("/dog/characteristic", {
            params: {
              dogId: this.dog.dogId
            }
          }
      ).then(response => {
        this.characteristics = response.data
        this.displaySelectedDogData = true
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    },

    selectDog: function (dog) {
      this.dog = dog
      this.displayDogsInfoView = false
      this.getDogsInfo()
    },

    displayMainViewDiv: function () {
      this.displayMainView = true
      this.displayDogsInfoView = false

    },

    displayDogsInfo: function () {
      this.hideAllDivs()
      this.displayDogsInfoView = true
    },

    hideAllDivs: function () {
      this.displayMainView = false

    },

    displayGetDogsInfo: function () {

    },

    getAllDogs: function () {
      this.$http.get("/dog/get/all")
          .then(response => {
            this.dogs = response.data
            this.displayDogsInfo()
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