<template>
    <div class="container mt-3">
        <h2>CoreHunter 3</h2>
        <hr>
        <b-card>
            <b-overlay :show="show" rounded="sm" >
            <b-card-text class="mb-3">
                <b-badge href="#" variant="info">Info</b-badge>
                <p>Core Hunter is a flexible tool to sample diverse, representative subsets from large germplasm collections, with minimum redundancy. Such so-called core collections have applications in plant breeding and genetic resource management in general. Core Hunter can construct cores based on genetic marker data, phenotypic traits or precomputed distance matrices, optimizing one of many provided evaluation measures depending on the precise purpose of the core (e.g. high diversity, representativeness, or allelic richness). In addition, multiple measures can be simultaneously optimized as part of a weighted index to bring the different perspectives closer together.</p>


                <b-card header="Data Set" header-bg-variant="dark" header-text-variant="white">
                    <b-row>
                        <b-col md="4" sm="6">
                            <b-form-group label="Add Dataset">
                                <b-form-file v-model="input.dataset" accept=".tsv, .csv, .txt" class="mt-1 mb-3" plain></b-form-file>
                            </b-form-group>
                        </b-col>
                    </b-row>
                </b-card>

                <b-card header="Arguments" class="mt-3" header-bg-variant="dark" header-text-variant="white">
                    <b-row>
                        <b-col md="4" sm="6">
                            <b-form-group>
                                <label for="sb-core">Core Size </label>
                                <b-form-spinbutton id="sb-core" v-model="input.core" min="5" class="mb-2" inline></b-form-spinbutton>
                            </b-form-group>
                        </b-col>
                        <b-col md="4" sm="6">
                            <b-form-group>
                                <label for="sb-mode">Mode </label>
                                <b-form-select id="sb-mode" v-model="input.mode" :options="modes"></b-form-select>
                            </b-form-group>
                        </b-col>
                        <b-col md="4" sm="6">
                            <b-form-group>
                                <label for="sb-time">Time (secs) </label>
                                <b-form-spinbutton id="sb-time" v-model="input.time" min="0" max="10" class="mb-2" inline></b-form-spinbutton>
                            </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col md="4">
                            <b-form-group
                                label="Always Selected"
                                description="Items that should always be selected in the core collection"
                            >
                                <b-form-input
                                    id="input-formatter"
                                    placeholder="item1, item2, item3 ...."
                                    v-model="input.always"
                                ></b-form-input>
                            </b-form-group>
                        </b-col>
                        <b-col md="4">
                            <b-form-group
                                label="Never Selected"
                                description="Items that should never be selected in the core collection"
                            >
                                <b-form-input
                                    id="input-formatter"
                                    placeholder="item1, item2, item3 ...."
                                    v-model="input.never"
                                ></b-form-input>
                            </b-form-group>
  
                        </b-col>
                    </b-row>
                </b-card>
                
                <b-card header="Objetives" class="mt-3" header-bg-variant="dark" header-text-variant="white">
                    <b-row>
                        <b-col md="4">
                            <b-form-group label="Objetive">
                                <b-form-select v-model="type" :options="objetives"></b-form-select>
                            </b-form-group>
                        </b-col>
                        <b-col md="4">
                            <b-form-group label="Measure">
                                <b-form-select v-model="measure" :options="measures"></b-form-select>
                            </b-form-group>
                        </b-col>
                        <b-col md="1">
                            <b-form-group label="Weight (%)">
                                <b-form-input v-model="weight" size="sm"></b-form-input>
                            </b-form-group>
                        </b-col>
                        <b-col md="12">
                            <b-form-group class="pt-1">
                                <b-button-group size="sm">
                                    <b-button @click="addObjetive" variant="primary">Add Objetive</b-button>
                                    <b-button @click="removeObjetive" variant="danger">Remove Objetive</b-button>
                                </b-button-group>
                                <!-- <b-button @click="addObjetive" variant="primary" size="sm">Add Objetive</b-button> -->
                            </b-form-group>
                        </b-col>
                    </b-row>

                    <b-row class="mt-3">
                        <!-- {{input.objetives}} -->
                        
                        <b-col md="12">
                            <table class="table table-bordered  table-sm">
                                <thead>
                                    <tr>
                                        <th scope="col">#</th>
                                        <th scope="col">Objetive</th>
                                        <th scope="col">Measure</th>
                                        <th scope="col">Weight</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in input.objetives" :key=index> <!-- Recorremos nuestro array -->
                                        <td v-text="index +1"></td> <!--En la primera columna mostramos el nombre-->
                                        <td v-text="item.type"></td> <!--En la primera columna mostramos el nombre-->
                                        <td v-text="item.measure"></td> <!--En la primera columna mostramos el nombre-->
                                        <td v-text="item.weight"></td> <!--En la segunda mostramos el apellido-->
                                    </tr>                       
                                </tbody>
                            </table>
                        </b-col>
                        
                    </b-row>                 
                </b-card>
                
            </b-card-text>
            <template v-slot:overlay>
                <div class="text-center">
                    <b-icon icon="stopwatch" font-scale="3" animation="cylon"></b-icon>
                    <p class="text-center"><b>Runing CoreHunter 3<br>Please wait...</b></p>
                </div>
            </template>
            </b-overlay>
           
            <hr>
            <b-button @click="corehunter" variant="primary" size="sm">Run CoreHunter</b-button>
            
        </b-card>
        <hr>
        <!-- RESULTADOS -->
        <div class="resultados" v-if="show_result">

             <b-card
                border-variant="light"
                header="Resultado"
                header-bg-variant="success"
                header-text-variant="white"
            >
                <b-card-text>
                    
                </b-card-text>
            </b-card>
        </div>     
    </div>
</template>

<script>
    export default {
        data(){
            return {
                show: false,
                show_result: false,
                input: {
                    dataset: null,
                    core: 10,
                    mode: 'default',
                    time: 5,
                    always: '',
                    never: '',
                    objetives: []
                }, 
                
                modes: [
                    { value: 'default', text: 'default'},
                    { value: 'fast', text: 'fast'}
                ],
                objetives: [
                    { value: 'EN', text: '(EN) Average entry-to-nearest-entry distance'},
                    { value: 'AN', text: '(AN) Average accession-to-nearest-entry distance' },
                    { value: 'EE', text: '(EE) Average entry-to-entry distance' },
                    { value: 'SH', text: '(SH) Shannon’s allelic diversity index'},
                    { value: 'HE', text: '(HE) Expected proportion of heterozygous loci.'},
                    { value: 'CV', text: '(CV) Allele coverage'}
                ],
                measures: [
                    { value: 'MR', text: '(MR) Modified Rogers distance'},
                    { value: 'CE', text: '(CE) Cavalli-Sforza and Edwards distance'},
                    { value: 'GD', text: '(GD) Gower distance'},
                    { value: 'PD', text: '(PD) Precomputed distances'}
                ],
                type: 'EN',
                measure: 'MR',
                weight: 100
            }
        },

        methods: {
            
            async corehunter(){
                try {
                    if(this.input.dataset != null){
                        this.show = true
                        this.show_result = false
                        this.show_result = true
                        this.show = false
                    
                    }else{
                        console.log("Faltan parametros")
                    }
                   
                } catch (error) {
                    console.log(error)
                }
            },

            addObjetive(){

                let weight
                

                /* if (this.input.objetives.length === 0) { weight = 100}
                if (this.input.objetives.length === 1) { 
                    this.input.objetives[0].weight = 50
                    weight = 50
                }
                if (this.input.objetives.length === 2) { 
                    this.input.objetives[0].weight = 33.3
                    this.input.objetives[1].weight = 33.3
                    weight = 33.3
                }
                if (this.input.objetives.length === 3) { 
                    this.input.objetives[0].weight = 25
                    this.input.objetives[1].weight = 25
                    this.input.objetives[2].weight = 25
                    weight = 25
                }
                if (this.input.objetives.length === 4) { 
                    this.input.objetives[0].weight = 20
                    this.input.objetives[1].weight = 20
                    this.input.objetives[2].weight = 20
                    this.input.objetives[3].weight = 20
                    weight = 20
                } */
                 
                let obj = {
                    type: this.type,
                    measure: this.measure,
                    weight 
                }     

                this.input.objetives.push(obj)
            },

            removeObjetive(){
                this.input.objetives.pop()
            }
        }
    }
</script>

<style scoped>

.resultados{
    height: 450px;
}

</style>