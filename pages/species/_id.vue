<template>
    <div class="container mt-3">
        <h2>{{genoma.code}}</h2>
        <hr>
         <client-only>
        <b-card no-body>
            <b-tabs pills card vertical>

                <b-tab title="Resumen" active>
                    <b-card-text>
                        <h4>Visión general</h4>
                        <hr>
                        <table class="table table-striped table-sm">
                              <thead>
                                <tr>
                                <th scope="col">Codigo</th>
                                <th scope="col">Grupo taxonomico</th>
                                <th scope="col">Version</th>
                                <th scope="col">Level</th>
                                <th scope="col">#Contigs</th>
                                <th scope="col">Size</th>
                                <th scope="col">Genes</th>
                                <th scope="col">mRNA</th>
                                <th scope="col">rRNA</th>
                                <th scope="col">tRNA</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <th scope="row">{{genoma.code}}</th>
                                    <td>{{genoma.group}}</td>
                                    <td>{{genoma.version}}</td>
                                    <td>{{genoma.level}}</td>
                                    <td>{{genoma.contig}}</td>
                                    <td>{{genoma.size}}</td>
                                    <td>{{genoma.genes}}</td>
                                    <td>{{genoma.cds}}</td>
                                    <td>{{genoma.rRNA}}</td>
                                    <td>{{genoma.tRNA}}</td>
           
                                </tr>
                            </tbody>
                        </table>
                        <hr>
                        <p><strong>Descripción</strong></p>
                        <p></p>
                        <hr>
                        <p><strong>Secuanciacion, Ensamble y  Anotacióm</strong></p>
                        {{genoma.methods}}

                    </b-card-text>
                </b-tab>

                <b-tab title="Homología">
                    <b-card-text>
                        <h4>Homología</h4>
                        <hr>
                        <p>Homology of the {{genoma.code}} proteins was determined by pairwise sequence comparison using the blastp algorithm against protein databases. An expectation value cutoff less than 1e-9 was used for the NCBI nr (Release 2018-05) and 1e-6  for UniProtKB/SwissProt (Release 2019-01)databases. The best hit reports are available for download in csv format.</p>
                        <p><strong> Downloads</strong></p>
                        <div class="row">
                            <div class="col-md-4">Blast results </div>
                            <div class="col-md-8">
                                <b-link :href="`ftp://${host}/Pseudomonas/${genoma.group}/${genoma.code}/blast/${genoma.code}_genomic_vs_nt.tsv`">Download</b-link>
                            </div>
                        </div> 
                    </b-card-text>
                </b-tab>
                <b-tab title="Anotación">
                    <b-card-text>
                        <h4>Anotación</h4>
                        <hr>
                        <p>Prokka finds and annotates features (both protein coding regions and RNA genes, i.e. tRNA, rRNA) present on on a sequence. Note, Prokka uses a two-step process for the annotation of protein coding regions: first, protein coding regions on the genome are identified using Prodigal; second, the function of the encoded protein is predicted by similarity to proteins in one of many protein or protein domain databases. Prokka is a software tool that can be used to annotate bacterial, archaeal and viral genomes quickly, generating standard output files in GenBank, EMBL and gff formats.<b-link :href="`ftp://${host}/Pseudomonas/${genoma.group}/${genoma.code}`">FTP repository.</b-link></p>
                    </b-card-text>
                </b-tab>
                <b-tab title="Análsis Funcional">
                    <b-card-text>
                        <h4>Análsis Funcional</h4>
                        <hr>
                        <p>Functional annotation for the {{genoma.code}} genome are available for download below. {{genoma.code}} proteins were analyzed using <a href="https://github.com/eggnogdb/eggnog-mapper" target="_blank">eggnog-mapper</a> in order to assign Gene Ontology (GO) terms. Pathways analysis was performed using the KEGG Automatic Annotation Server (KAAS).</p>
                        <p><strong> Downloads</strong></p>
                        <div class="row">
                            <div class="col-md-4">Functional annotation eggNOG</div>
                            <div class="col-md-8">
                                <b-link :href="`ftp://${host}/Pseudomonas/${genoma.group}/${genoma.code}/functional_analysis/${genoma.code}.emapper.annotations`">Download</b-link>
                            </div>
                        </div> 
                    </b-card-text>
                </b-tab>
                <b-tab title="Descargar">
                    <b-card-text>
                        <h4>Descargar</h4>
                        <hr>
                        <p>All assembly and annotation files are available for download. Each data type page will provide a description of the available files and links to download.  Alternatively, you can browse all available files on the 
                        <b-link :href="`ftp://${host}/Pseudomonas/${genoma.group}/${genoma.code}`">FTP repository.</b-link></p> 
                    </b-card-text>
                </b-tab>
            
            </b-tabs>        
        </b-card>
        </client-only>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                genoma: [],
                host: 'localhost',
                //host: '192.168.22.26'
            }   
        },
        async created(){
            try {
                const res = await this.$axios.get(`/assembly/get-assembly/${this.$route.params.id}`);
                this.genoma = res.data.result
            } catch (error) {
                console.log(error)
            }
        }
    }
</script>

<style>

</style>
