<template>
  <b-container class="mt-3">
    <b-card 
      class="portada1"
    >
      <b-card-text class="lead">
        <h3><b>Red Genómica y Bioinformática INIA</b></h3>
        <hr>
        <p class="text-center">Plataforma bioinformática con datos genómicos y herramientas de análisis para estudiar la variabilidad genética de las poblaciones de <i>Pseudomonas syringea</i> y nematodos agalladores (<i>Meloidogyne spp</i>), que afectan los frutales de Prunus en Chile. Para facilitar el desarrollo de herramientas de identificación de variantes (y/o especies) y estudios epidemiológicos para su control.</p>

        <b-container fluid class="p-4 bg-ligth">
          <b-row>
            <b-col>
              <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=54" alt="Image 1"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=58" alt="Image 2"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=59" alt="Image 3"></b-img>
            </b-col>
          </b-row>
        </b-container>

        <!-- <div class="imagenes mt-4">
          <b-row>
            <b-col>
              <b-img thumbnail fluid src="https://www.sciadnewswire.com/sites/default/files/images/slideshow/iStock_000019077670Medium.jpg" alt="Image 3"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="https://salud.ideal.es/wp-content/images/noticias/bioinfo.jpg" alt="Image 2"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="https://www.bbva.com/wp-content/uploads/2017/08/iStock-664868402-1024x683.jpg" alt="Image 3"></b-img>
            </b-col>
            <b-col>
              <b-img thumbnail fluid src="https://www.cabecera.mx/wp-content/uploads/2017/03/d66e89a1d1214ea7700754049bad4795.jpg" alt="Image 1"></b-img>
            </b-col>
          </b-row>
        </div> -->
        
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

.portada2{
    height: 300px;
    padding: 2%;
   
}
</style>