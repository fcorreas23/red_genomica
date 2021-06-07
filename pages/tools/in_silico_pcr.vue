<template>
    <div class="container mt-3">
        <h2><i>In silico</i> PCR</h2>
        <hr>
        <b-card>
            <b-overlay :show="show" rounded="sm" >
            <b-card-text class="mb-3">
                <p>This script searches nucleotide sequences using a set of primer sequences and attempts to anticipate the results of a PCR reaction using these same sequences. Results are based on sequence identity. Real-world factors such as melting temperature, annealing time, salt concentration, or secondary structure are not accounted for in this script.</p>
                <b-row>
                    <b-col md="4" sm="6">
                        <b-form-group
                            label="Organism"
                            label-for="organism"
                        >
                            <b-form-select id= "organism" v-model="input.seq">
                                <b-form-select-option :value="null" >Select a genome</b-form-select-option>
                                <b-form-select-option v-for="item in genomas" :key="item._id" :value="`${item.code}`">{{item.code}} {{item.specie.scientific_name}}</b-form-select-option>
                            </b-form-select>
                        </b-form-group>
                        <b-form-group
                            label="Target"
                            label-for="target"   
                        >
                            <b-form-select id= "target" v-model="input.target" :options="options"></b-form-select>
                        </b-form-group>
                    </b-col>
                    <b-col md="2" sm="6">
                        <b-form-group
                            label="Mismatches"
                            label-for="mismatch"   
                        >
                            <b-form-spinbutton v-model="input.mismatch" min="0" max="3" class="mb-2"></b-form-spinbutton>
                            <!-- <b-form-select id= "mismatch" v-model="input.mismatch" :options="options"></b-form-select> -->
                        </b-form-group>
                    </b-col>
                    <b-col md="3" sm="6">
                        <b-form-group
                            description="Orientation 5' → 3'"
                            label="Forward primer"
                            label-for="forward"
                        >
                            <b-form-input id="forward" v-model="input.forward"></b-form-input>
                        </b-form-group>
                    </b-col>
                    <b-col md="3" sm="6">
                        <b-form-group
                            description="Orientation 5' → 3'"
                            label="Reverse primer "
                            label-for="reverse"
                        >
                            <b-form-input id="reverse" v-model="input.reverse"></b-form-input>
                        </b-form-group>
                    </b-col>
                </b-row>
            </b-card-text>
            <template v-slot:overlay>
                <div class="text-center">
                    <b-icon icon="stopwatch" font-scale="3" animation="cylon"></b-icon>
                    <p class="text-center"><b>Runing in silico PCR<br>Please wait...</b></p>
                </div>
            </template>
            </b-overlay>
           
            <hr>
            <button @click="in_silico_pcr" class="btn btn-secondary btn-small mt-3">Run PCR</button>
        </b-card>
        <hr>
        <div class="resultados">

             <b-card
                border-variant="light"
                header="Result"
                header-bg-variant="success"
                header-text-variant="white"
                v-if="show_result"
            >
                <b-card-text>
                    <b-row>
                        <b-col md=4>
                            <b-table striped hover :items="resultado_pcr"></b-table>
                        </b-col>
                        <b-col md=8>
                            <textarea class="form-control" rows="10" v-model="resultado_amplicon"></textarea>
                        </b-col>
                    </b-row>
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
                    seq: null,
                    target: '_genomic.fna',
                    forward: 'ATGCACCCAGTCGTGATACA',
                    reverse: 'GTTGGCCAAGACACCTTCAT',
                    l: 3000,
                    mismatch : 0,

                },
                options: [
                    { value: '_genomic.fna', text: 'Draft genome' },
                    { value: '_rna_from_genomic.ffn', text: 'Genes' }
                ],
                genomas: [],
                resultado_pcr: '',
                resultado_amplicon: '',
            }
        },
        async created(){
            try {
                const res = await this.$axios.get('/assembly/list');
                this.genomas = res.data.result
            } catch (error) {
                console.log(error)
            }
        },
        computed: {
            syringae(){
                return this.genomas.filter(f => f.group == 'Pseudomonas_syringae_group')
            }
        },
        methods: {
            
            async in_silico_pcr(){
                try {
                    if(this.input.seq != null){
                        this.show = true
                        let res = await this.$axios.post('/biotools/in_silico_pcr/', this.input)
                        this.resultado_pcr = res.data.result.pcr_stats
                        this.resultado_amplicon = res.data.result.amplicon
                        this.show_result = true
                        this.show = false
                    }
                   
                } catch (error) {
                    console.log(error)
                }
            }
        }
    }
</script>

<style scoped>

.resultados{
    height: 450px;
}

</style>