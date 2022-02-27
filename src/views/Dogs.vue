<template>
  <div class="Dogs">
    <br>
    <div v-if="displayMainView">
      <h4>Soovime meie koertele parimat elu</h4>
      <h4>Tutvu meie koertaga
        <button type="button" class="btn btn-secondary" v-on:click="displayDogsInfo">siin</button></h4>
      <br>
      <img alt="Doggo"
           src="https://images.unsplash.com/photo-1537123547273-e59f4f437f1b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8YmljaG9ufGVufDB8fDB8fA%3D%3D&w=1000&q=80"
           class="img-fluid">
    </div>
    <div v-if="displayDogsInfoView">
      <h4></h4>
      <button type="button" class="btn btn-outline-primary btn-sm" v-on:click="displayMainViewDiv">Tagasi...</button>
      <br>

      <img alt="Muki" src="https://www.pdsa.org.uk/media/9808/gallery-1-bloodhound-sniffing.jpg"class="img-fluid">
      <p>Muki on 11-aastane vanahäära. Rahuliku loomuga ning pikalt varjupaigas olnud.Sattus varjupaika, seoses omaniku surmaga.</p>
      <img alt="Pontu" src="https://www.pdsa.org.uk/media/8275/border-terrier-cost-of-min.jpg"class="img-fluid">
      <p> Pontu on väike ja tore </p>
      <img alt="Fjodor" src=" https://www.pdsa.org.uk/media/8262/cocker-spaniel-looking-at-camera-min.jpg"class="img-fluid">
      <p>Fjodor on energiline noor koer, kes aeg-ajalt näksab.</p>
      <img alt="Nisu" src="https://www.petmd.com/sites/default/files/styles/article_image/public/yellow-lab-puppy-biting-and-tugging-on-leash_0.jpg"class="img-fluid">
      <p>Nisu on energiline, tirib tihti, sest vajab kannatlikust. </p>
      <img alt="Lotte" src="https://www.pdsa.org.uk/media/9904/dachshund-in-a-meadow.jpg"class="img-fluid">
      <p>Lotte on väike, väle ja klähvib palju. </p>
      <img alt="Max" src="https://www.petmd.com/sites/default/files/dog-running.jpg"class="img-fluid">
      <p>Max kipub hüppama ja on enamasti energiline. </p>
      <img alt="Bella" src=" https://www.pdsa.org.uk/media/10762/chihuahua-gallery-2-min.jpg"class="img-fluid">
      <p>Bella on väike ja tige. </p>

<!--      <table class="table table-hover">-->
<!--        <thead>-->
<!--        <tr>-->
<!--          <th scope="col">Koera nimi</th>-->
<!--          <th scope="col">Koera ID</th>-->
<!--          <th scope="col">Koera kirjeldus</th>-->
<!--          <th scope="col">Iseloomulikud jooned</th>-->
<!--        </tr>-->
<!--        </thead>-->
<!--        <tbody>-->
<!--        <tr v-for="row in characteristics">-->
<!--          <td>{{ row.dogName }}</td>-->
<!--          <td>{{ row.dogId }}</td>-->
<!--          <td>{{ row.dogDescription }}</td>-->
<!--          <td>{{ row.characteristicTag }}</td>-->

<!--        </tr>-->
<!--        </tbody>-->
<!--      </table>-->
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
      characteristicTags: {},
      characteristicTag: "",
      displayMainView: true,
      displayDogsInfoView: false

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
       this.dogs=response.data
        this.characteristicTag=response.data.characteristicTag
        this.hideAllDivs()
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
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
  }
}
</script>

<style scoped>

</style>