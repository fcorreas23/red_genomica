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
                                <b-form-input type="number" v-model="weight" size="sm"></b-form-input>
                            </b-form-group>
                        </b-col> 
                        <b-col md="12" class="mt-2">
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
                                        <td v-text="item[0]"></td> <!--En la primera columna mostramos el nombre-->
                                        <td v-text="item[1]"></td> <!--En la primera columna mostramos el nombre-->
                                        <td v-text="item[2]"></td> <!--En la segunda mostramos el apellido-->
                                    </tr>                       
                                </tbody>
                            </table>
                        </b-col>
                        
                    </b-row>                 
                </b-card>
                <b-alert  :show="message.dismissCountDown" dismissible :variant="message.color" @dismissed="message.dismissCountDown=0" @dismiss-count-down="countDownChanged"> {{message.text}}</b-alert>
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
                    {{input}}
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
                    always: null,
                    never: null,
                    objetives: []
                },
                type: 'EN',
                measure: 'MR',
                weight: 100,
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

                message: {
                    color: '',
                    text: '',
                    dismissSecs: 3,
                    dismissCountDown: 0
                }
            }
        },

        methods: {
            
            async corehunter(){
                try {
                    if(this.input.dataset != null){
                        
                        console.log(this.input.objetives.length)
                        
                        let formData = new FormData;
                        formData.append('file',this.input.dataset)
                        formData.append('core', this.input.core)
                        formData.append('mode', this.input.mode)
                        formData.append('time', this.input.time)
                        formData.append('always', this.input.always)
                        formData.append('never', this.input.never)
                        formData.append('objetives', this.input.objetives)

                        
                        this.show = true
                        this.show_result = false
                        //console.log(formData)
                        let res = await this.$axios.post('/biotools/corehunter', formData)
                        console.log(res)

                        this.show_result = true
                        this.show = false
                    
                    }else{
                        this.message.text = "Ingrese set de datos"
                        this.message.color = "warning"
                        this.showAlert()
                        console.log("Faltan parametros")
                    }
                   
                } catch (error) {
                    console.log(error)
                }
            },

            addObjetive(){
                
                let objetives = this.input.objetives
                let sum = 0
                let x = 0                        
                
                if (Array.isArray(objetives) && objetives.length) {
                    
                    //objetives.forEach(element => console.log(element.weight));

                    objetives.map(element => {sum += element[2]})

                    x = parseInt(sum) + parseInt(this.weight)

                    if(x <= 100 && sum < 100) {
                        objetives.push([this.type, this.measure, this.weight])
                        
                    }else{
                        this.message.text = "Ya no se puede agregar mas objetivos"
                        this.message.color = "warning"
                        this.showAlert()
                        console.log("Ya no se puede agregar mas objetivos")
                    }          
                            
                }else{                   
                    objetives.push([this.type, this.measure, this.weight])           
                    this.weight = 100 - this.weight
                }                                            
            },

            removeObjetive(){
                let sum = 0
                this.message.show = false

                this.input.objetives.pop()
                this.input.objetives.map(element => {sum += element.weight})
                this.weight = 100 - sum

            },

            countDownChanged(dismissCountDown) {
                this.message.dismissCountDown = dismissCountDown
            },

            showAlert() {
                this.message.dismissCountDown = this.message.dismissSecs
            }
        }
    }
</script>

<style scoped>

.resultados{
    height: 450px;
}

</style>