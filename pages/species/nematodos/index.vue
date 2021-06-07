<template>
    <div class="container mt-3">
        <h2>Meloidogyne</h2>
        <hr>
        <b-card no-body>
            <b-tabs pills card vertical>
                <b-tab title="Description" active>
                    <b-card-text>
                        <hr>
                        <b-row>
                            <b-col sm="12" md="9">
                                 <p class="text-justify"><i>Meloidogyne</i> spp. es uno de los géneros de nematodos más importantes debido a las pérdidas que produce en los cultivos, ya que induce alteraciones en las raíces provocando agallas. Para esto, rompe las células de la planta, disuelve las paredes celulares o induce cambios fisiológicos en los tejidos radicales como resultado de la inyección de sustancias fitotóxicas a través de su estilete. Estas afecciones provocan una predisposición de la planta al ataque de otros microorganismos patógenos como hongos, bacterias y virus que penetran la planta a través de las heridas ocasionadas por el nematodo. Se trata de especies endoparásitas obligadas, capaces de adaptarse a una gran cantidad de plantas huéspedes y con una elevada tasa de reproducción con varias generaciones por año.</p>
                            </b-col>
                            <b-col sm="12" md="3">
                                <b-img thumbnail fluid src="~/assets/img/Meloidogyne.jpg" alt="Responsive image"></b-img>
                            </b-col>
                        </b-row>
                       
                    </b-card-text>
                </b-tab>
                <b-tab title="Genomas">
                    <b-card-text>
                        <hr>
                        <h5><b>Genomas de referencias</b></h5>
                        <ul class="list-group">
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/281" target="blank">Meloidogyne incognita</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/86518" target="blank">Meloidogyne luci</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/36319" target="blank">Meloidogyne enterolobii</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/33826" target="blank">Meloidogyne floridensis</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/14464" target="blank">Meloidogyne graminicola</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/10912" target="blank">Meloidogyne javanica</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/10910" target="blank">Meloidogyne chitwoodi</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/10908" target="blank">Meloidogyne arenaria</a></i></li>
                            <li class="list-group-item"><i><a href="https://www.ncbi.nlm.nih.gov/genome/260" target="blank">Meloidogyne hapla</a></i></li>
                            
                        </ul>
                    </b-card-text>
                </b-tab>          
                
                <!-- <b-tab title="Descargar">
                    <b-card-text>
                        <h4>Descargar</h4>
                        <hr>
                        <p>All assembly and annotation files are available for download by selecting the desired data type in the left-hand "Resources" side bar.  Each data type page will provide a description of the available files and links to download.  Alternatively, you can browse all available files on the 
                        <a  :href="`ftp://${host}/`">FTP repository.</a> </p>
                    </b-card-text>
                </b-tab> -->
            </b-tabs>
        </b-card>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                genomas: [],
                //host: 'localhost',
                host: '192.168.22.26',
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>