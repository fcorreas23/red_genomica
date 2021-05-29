<template>
    <div class="container mt-3">
        <h2>Busqueda anotacion funcional</h2>
        <hr>
        <b-overlay :show="show_search" rounded="sm" >
            <b-card class="mb-2">
                <b-card-text>
                    <p>We also recommend, as a complement to searching the annotations, that you search, using BLAST , for homologs to a protein of interest, to find additional genes/proteins of interest that may have annotations that don't show up in your annotation search (i.e. if you want all homologs of a particular membrane protein, but the particular membrane protein has been named differently by different Genomes Projects). </p>
                    <b-form-group description="eg. map04978, ko:K07089, GO:0003674, 6.1.1.21, M00359">
                        <b-input-group class="mt-3">
                            <b-form-input v-model="buscar" size="sm"></b-form-input>
                            <b-input-group-append>
                                <b-button variant="info" size="sm" @click="search">Buscar</b-button>
                            </b-input-group-append>
                        </b-input-group>
                    </b-form-group>
                </b-card-text>
            </b-card>       
            <template v-slot:overlay>
                <div class="text-center">
                    <b-icon icon="stopwatch" font-scale="3" animation="cylon"></b-icon>
                    <p class="text-center"><b>Searching<br>Please wait...</b></p>
                </div>
            </template>
        </b-overlay> 
        
            
        <b-card border-variant="light" header="RESULTADOS" :header-bg-variant="status" header-text-variant="white" v-if="show">
            <b-card-text>

                    <!-- <b-pagination
                        v-model="currentPage"
                        :total-rows="rows"
                        :per-page="perPage"
                        aria-controls="my-table"
                    ></b-pagination> -->
                    <b-table-simple id="my-table" hover small caption-top responsive>
                        <caption>{{message}} in database INIA, grouped by text: {{buscar}}</caption>
                        <b-thead head-variant="dark">
                            <b-tr>
                                <b-th>seqid</b-th>
                                <b-th>Descripción</b-th>
                                <b-th>Taxonomic group</b-th>
                                <b-th>Preferred name</b-th>
                                <b-th><a href="http://clovr.org/docs/clusters-of-orthologous-groups-cogs/" target="_blank">Funcional COG</a></b-th>
                            </b-tr>
                        </b-thead>
                        <b-tbody>
                            <b-tr  v-for="resultado in resultados" :key="resultado.id">
                            <b-td><b-button class="btn btn-light text-dark" @click="getSecuencia(`${resultado.locus}`)"> {{resultado.locus}}</b-button></b-td>
                            <b-td>{{resultado.description}}</b-td>
                            <b-td>{{resultado.assembly.group}}</b-td>
                            <b-td>{{resultado.alias}}</b-td>
                            <b-td>{{resultado.cog_funcional}}</b-td>
                            </b-tr>
                        </b-tbody>
                    </b-table-simple>
            </b-card-text>        
        </b-card>

        <b-modal 
            v-model="show_modal"
            title="Gen"
            :header-bg-variant= "headerBgVariant"
            :header-text-variant="headerTextVariant"
            size="lg" 
            ok-only 
            scrollable 
            v-if="show_modal"
        >
            <b-card>
                
                <div class="row">
                    <div class="col-md-2 border-right">ID</div>
                    <div class="col-md-10">{{secuencia.protein.locus}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right">Nucleotide seq.</div>
                    <div class="col-md-10"> {{secuencia.nucl.sequence}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right">Protein seq.</div>
                    <div class="col-md-10"> {{secuencia.protein.sequence}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right">Descripcion</div>
                    <div class="col-md-10">{{secuencia.protein.description}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right"> Preferred name</div>
                    <div class="col-md-10">{{secuencia.protein.alias}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right">GOs</div>
                    <div class="col-md-10">{{secuencia.protein.gene_ontology}}</div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-md-2 border-right">KEGG pathway</div>
                    <div class="col-md-10">{{secuencia.protein.kegg_pathway}}</div>
                </div>
            </b-card>
        </b-modal>
    </div>
</template>

<script>
    export default {
        data(){
          return {
              headerBgVariant: 'primary',
              headerTextVariant: 'light',
              show: false,
              show_modal: false,
              show_search: false,
              buscar: '',
              status: '',
              message: '',
              resultados:[],
              secuencia: {},
              seqnucl: [],
              //perPage: 20,
              //currentPage: 1,
          }
        },
        computed: {
            rows() {
                return this.resultados.length
            }
        },    
      
        methods: {
          async search(){
              try {
                this.show_search = true
                this.show = false
                let res = await this.$axios.get('/protein/functional_annotation/'+this.buscar)
                this.show_search= false,
                this.show = true,
                this.status = res.data.status
                this.message = res.data.msg
                this.resultados = res.data.result
                

              } catch (error) {
                  console.log(error)
              }
          },

          async getSecuencia(id){
             try {
                 const res = await this.$axios.get('/protein/getseq/'+id)
                 this.secuencia = res.data.result
                 this.show_modal = true;

            } catch (error) {
                console.log("ERROR:",error)
            }
          }
      }  
    }
</script>

<style>

</style>