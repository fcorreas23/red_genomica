<template>
    <div class="container mt-3">
        <h2>Herraminetas externas</h2>
        <hr>
        <b-row>
          <b-col md="4">
            <b-list-group>
              <b-list-group-item variant="primary"><h4>Diagramas de Venn</h4></b-list-group-item>
              <b-list-group-item href="https://bioinfogp.cnb.csic.es/tools/venny/" target="_blank" class="flex-column align-items-start">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">Venny</h5><small>2.1</small>
                </div>
                <p class="mb-1">Oliveros, J.C. (2007-2015) Venny. An interactive tool for comparing lists with Venn's diagrams.</p>
              </b-list-group-item>

              <b-list-group-item href="http://bioinformatics.psb.ugent.be/webtools/Venn/" target="_blank" class="flex-column align-items-start">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">Venn Diagrams</h5>
                </div>

                <p class="mb-1">
                  With this tool you can calculate the intersection(s) of list of elements. It will generate a textual output indicating which elements are in each intersection or are unique to a certain list.
                </p>
              </b-list-group-item>

              <b-list-group-item href="http://www.interactivenn.net/" target="_blank" class="flex-column align-items-start">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">InteractiVenn</h5>
                </div>
                <p class="mb-1">
                  Heberle, H.; Meirelles, G. V.; da Silva, F. R.; Telles, G. P.; Minghim, R. InteractiVenn: a web-based tool for the analysis of sets through Venn diagrams. BMC Bioinformatics 16:169 (2015).
                </p>
              </b-list-group-item>
            </b-list-group>
          </b-col>

          <b-col md="4">
            <b-list-group>
              <b-list-group-item variant="primary"><h4>Análisis GO</h4></b-list-group-item>
              <b-list-group-item href="http://systemsbiology.cau.edu.cn/agriGOv2/" target="_blank" class="flex-column align-items-start">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">AgriGO</h5><small>2.0</small>
                </div>
                <p class="mb-1">AgriGO is a web-based tool and database for gene ontology analyses. It specifically focuses on agricultural species and is user-friendly. AgriGO is designed to provide deep support to the agricultural community in the realm of ontology analyses. </p>
              </b-list-group-item>

              <b-list-group-item href="http://bioinformatics.psb.ugent.be/webtools/Venn/" target="_blank" class="flex-column align-items-start">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">WEGO</h5><small>2.0</small>
                </div>
                <p class="mb-1">
                 WEGO (Web Gene Ontology Annotation Plot) is a simple but useful tool for visualizing, comparing and plotting GO (Gene Ontology) annotation results.
                </p>
              </b-list-group-item>
            </b-list-group>
          </b-col>

          <b-col md="4">

          </b-col>
          <b-col md="4">

          </b-col>
        </b-row>
      

    </div>
</template>

<script>
    export default {
        data() {
            return {
                
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>