<template>
  <b-container class="mt-3">
    <b-card 
      class="portada1 shadow p-3 mb-5 bg-white rounded"
    >
      <b-card-text class="lead">
        <h3><b>Red Genómica y Bioinformática INIA</b></h3>
        <hr>
        <p class="text-center">Plataforma bioinformática con datos genómicos y herramientas de análisis para estudiar la variabilidad genética de las poblaciones de <i>Pseudomonas syringea</i> y nematodos agalladores (<i>Meloidogyne spp</i>), que afectan los frutales de Prunus en Chile. Para facilitar el desarrollo de herramientas de identificación de variantes (y/o especies) y estudios epidemiológicos para su control.</p>

        <b-container fluid class="p-4 bg-dark">
          <b-row>
            <b-col>
              <b-img thumbnail fluid src="~/assets/img/1.jpg" alt="Responsive image"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="~/assets/img/2.jpg" alt="Responsive image"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="~/assets/img/3.jpg" alt="Responsive image"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="~/assets/img/4.jpg" alt="Responsive image"></b-img>
            </b-col>
          </b-row>
        </b-container>
      </b-card-text>
    </b-card>
  </b-container>
</template>

<script>

export default {
  name: 'Home'
}
</script>

<style scoped>
.portada1{
    background-color: whitesmoke;
   
}
</style>