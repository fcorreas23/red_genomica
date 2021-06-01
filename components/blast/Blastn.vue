<template>
    <b-overlay :show="show" rounded="sm" >
    <div id="blastn">
        <b-card-text>Search a nucleotide database using a nucleotide query.</b-card-text>

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
            <p><small>Choose from one of the nucleotide BLAST databases listed below. </small></p>
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
        <button @click="blast" class="btn btn-secondary">BLAST</button>
        <hr>
        <b-card border-variant="light" header-bg-variant="success" header-text-variant="white" header="Resultados" class="mt-3" v-if="show_load" >
            <div class="d-flex justify-content-center mb-3">
                <b-card-text>
                    <b-table striped hover  :items="resultados"></b-table>
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
                show_load: false,
                input: {
                    type: 'blastn',
                    sequences: '>example\nATGAATGTCATTGAAAAAGCCCCGATCGGCAGCACGCTGCTGCAGGCCCAGTCGCCCTGG\nCGACGCGTGTTCACCGAGTTCTTCAGCTCGCCCACGGCGGTGACGGGCCTGGTGGTCCTG\nCTGATCATTGTGCTGGTTGCAGCCCTGGCGCCGTGGATTGTGGTGCAGAACCCCTACGAC\nCTGATGCAGCTCAACGTCATGGACGCGCGCATGCCGCCTGGCAGCCTCAATGGTGACAGC\nGGTTACACCTATTGGCTGGGCACAGACGGGCAGGGCCGCGATCTGGTCTCGGCGATTATC\nTACGGGCTGCGCATCAGCCTGTGGGTCGGCATTGGCTCGGCGTTGATTGCGGCAGTGCTC\nGGTACGCTGCTCGGGCTGGTATCTGCGTATGTCGGCGGCTGGCTCGATGCCTTGTTGATG\nCGTCTGGTCGACCTGCTGCTGTCGTTCCCGGTGATTCTCATGGCGCTGATGATCCTTGCC\nTGGCTGGGCAAGGGTGTCGGCAACGTGATGCTGACCCTGATAGTGCTCGAATGGGCCTAT\nTACGCGCGCACTGCACGCGGTCAGGCGCTGACCGAAAGCCGCCGTGAATACGTCGATGCG\nGCGCGCGGGCAGGGCGTGGGGCCGCTGCGTATTGTGGTTGGCCACATTCTGCCCAACTGC\nCTGCCACCGCTGATCGTGATCGGCGCCTTGCAGATTGCCCGCGCCATCACGCTGGAAGCG\nACCCTGTCGTTTCTTGGCCTGGGCGTGCCGGTCACCGAGCCGTCGCTGGGGCTGCTGATT\nGCCAACGGCTTTCAATACATGCTCAGCAATGAATACTGGATCAGTCTGTTTCCGGGCCTG\nGCGCTGTTGATCACGATCGTTGCAATCAATCTGGTCGGTGACCGTTTGCGCGATGTCCTG\nAACCCGAGGTTACAGCGATGA',
                    database: null,
                    max_target_seqs: 20,
                    evalue: 10.0
                },
                options: [
                    { value: null, text: 'Please select a database' },
                    { value: '/blastdb/nucl/labinia', text: 'INIA mRNA database' },
                    { value: '/blastdb/nucl/references_transcripts', text: 'NCBI mRNA database' }
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
                        this.show_load = false
                        let res = await this.$axios.post('/biotools/blast',this.input)
                        this.show_load = true
                        this.show = false
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

<style>

</style>