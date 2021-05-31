<template>
    <b-overlay :show="show" rounded="sm" >
    <div id="blastp">
        <b-card-text>Search protein database using a nucleotide query.</b-card-text>
        
        <b-card border-variant="light" header-bg-variant="primary" header-text-variant="white" header="Enter Nucleotide Query Sequence">
            <p><small>Enter one or more queries in the top text box. The data must be in FASTA format.</small></p>
            <b-card-text>
                <b-form-textarea
                    id="textarea-rows"
                    rows="8"
                    v-model="input.sequences"
                    style="font-size: 10pt"
                ></b-form-textarea>
            </b-card-text>
        </b-card>

        <b-card border-variant="light" header-bg-variant="primary" header-text-variant="white" header="Choose Search Target" class="mt-2">
            <p><small>Choose from one of the protein BLAST databases listed below. </small></p>
            <b-form-select v-model="input.database" :options="options"></b-form-select>
        </b-card>

        <b-alert
            :show="dismissCountDown"
            dismissible
            :variant="mensaje.color"
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged"
        >
            {{mensaje.text}}
        </b-alert>
        <button @click="blast" class="btn btn-secondary mt-3">BLAST</button>

        <hr>

        <b-card border-variant="light" header-bg-variant="success" header-text-variant="white" header="Resultados" class="mt-3" v-if="show_result" >
            <div class="d-flex justify-content-center mb-3">
                <b-card-text>
                        <table class="table table-responsive table-hover table-sm">
                            <thead class="thead-secondary">
                                <tr>
                                    <th>Query id </th>
                                    <!-- <th>Qlength</th> -->
                                    <th>Subject id</th>
                                    <th>Slength</th>
                                    <th>Description</th>
                                    <th>Per. Iden</th>
                                    <th>Query Coverage </th>
                                    <th>Alignment</th>
                                    <!-- <th>Mismatches</th> -->
                                    <!-- <th>Gap openings</th> -->
                                    <th>E-value</th>
                                   <!--  <th>Bitscore</th> -->
                                </tr>
                            </thead>
                              <tbody>
                                <tr v-for="(item,i) in resultados" :key="i">
                                    <td>{{item.qseqid}}</td>
                                   <!--  <td>{{item.qlen}}</td> -->
                                    <td><nuxt-link :to="`/tools/seq/${item.sseqid}`" class="btn btn-light">{{item.sseqid}}</nuxt-link></td>
                                    <td>{{item.slen}}</td>
                                    <td>{{item.stitle}}</td>
                                    <td>{{item.pident}}</td>
                                    <td>{{item.qcovs}}</td>
                                    <td>{{item.length}}</td>
                                    <!-- <td>{{item.mismatch}}</td>
                                    <td>{{item.gapopen}}</td> -->
                                    <td>{{item.evalue}}</td>
                                   <!--  <td>{{item.bitscore}}</td> -->

                                </tr>
                            </tbody>                        
                        </table>
                    <!--<b-table striped hover  :items="resultados"></b-table>-->
                </b-card-text>
                    
            </div>  
        </b-card>
    </div>
    <template v-slot:overlay>
        <div class="text-center">
            <b-icon icon="stopwatch" font-scale="3" animation="cylon"></b-icon>
            <p class="text-center"><b>Runing Blastp <br>Please wait...</b></p>
        </div>
    </template>
    </b-overlay>
</template>

<script>
    export default {
        data(){
            return {
                show: false,
                show_result: false,
                input: {
                    type: 'blastx',
                    sequences: '>example\nATGAATGTCATTGAAAAAGCCCCGATCGGCAGCACGCTGCTGCAGGCCCAGTCGCCCTGG\nCGACGCGTGTTCACCGAGTTCTTCAGCTCGCCCACGGCGGTGACGGGCCTGGTGGTCCTG\nCTGATCATTGTGCTGGTTGCAGCCCTGGCGCCGTGGATTGTGGTGCAGAACCCCTACGAC\nCTGATGCAGCTCAACGTCATGGACGCGCGCATGCCGCCTGGCAGCCTCAATGGTGACAGC\nGGTTACACCTATTGGCTGGGCACAGACGGGCAGGGCCGCGATCTGGTCTCGGCGATTATC\nTACGGGCTGCGCATCAGCCTGTGGGTCGGCATTGGCTCGGCGTTGATTGCGGCAGTGCTC\nGGTACGCTGCTCGGGCTGGTATCTGCGTATGTCGGCGGCTGGCTCGATGCCTTGTTGATG\nCGTCTGGTCGACCTGCTGCTGTCGTTCCCGGTGATTCTCATGGCGCTGATGATCCTTGCC\nTGGCTGGGCAAGGGTGTCGGCAACGTGATGCTGACCCTGATAGTGCTCGAATGGGCCTAT\nTACGCGCGCACTGCACGCGGTCAGGCGCTGACCGAAAGCCGCCGTGAATACGTCGATGCG\nGCGCGCGGGCAGGGCGTGGGGCCGCTGCGTATTGTGGTTGGCCACATTCTGCCCAACTGC\nCTGCCACCGCTGATCGTGATCGGCGCCTTGCAGATTGCCCGCGCCATCACGCTGGAAGCG\nACCCTGTCGTTTCTTGGCCTGGGCGTGCCGGTCACCGAGCCGTCGCTGGGGCTGCTGATT\nGCCAACGGCTTTCAATACATGCTCAGCAATGAATACTGGATCAGTCTGTTTCCGGGCCTG\nGCGCTGTTGATCACGATCGTTGCAATCAATCTGGTCGGTGACCGTTTGCGCGATGTCCTG\nAACCCGAGGTTACAGCGATGA',
                    database: null,
                    max_target_seqs: 20,
                    evalue: 10.0
                },
                options: [
                    { value: null, text: 'Please select a database' },
                    { value: '/blastdb/prot/labinia', text: 'INIA database' }
                ],
                resultados: [],
                mensaje: {
                    color: '',
                    text: ''
                }, 
                dismissSecs: 5,
                dismissCountDown: 0
            }   
        },
        methods: {
            async blast(){

                if(this.input.sequences == '' || this.input.database == null){
                    this.mensaje.color = 'danger'
                    this.mensaje.text = 'Select database or paste sequence'
                    this.showAlert()
                }else{
                    try {
                        this.show = true
                        this.show_result = false
                        let res = await this.$axios.post('/biotools/blast',this.input)
                        this.show = false
                        this.show_result = true
                        this.resultados = res.data.result
                    } catch (error) {
                        console.log("Error", error)
                    }
                }
            },
            countDownChanged(dismissCountDown) {
                this.dismissCountDown = dismissCountDown
            },

            showAlert() {
                this.dismissCountDown = this.dismissSecs
            }
        }        
    }
</script>