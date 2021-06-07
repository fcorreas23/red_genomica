<template>
    <div id="menu">
       <b-navbar toggleable="lg" type="dark" variant="primary">
           <b-container>
               <b-navbar-brand to="/">                   
                    <b>RedGBioINIA</b>
                </b-navbar-brand>
               <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
               
               <b-collapse id="nav-collapse" is-nav>

                   <b-navbar-nav>
                        <b-nav-item-dropdown text="Especies" right>
                           <b-dropdown-item to="/species/pseudomonas"><em>Pseudomonas syringae</em></b-dropdown-item>
                           <b-dropdown-item to="/species/nematodos">Nematodos</b-dropdown-item>
                        </b-nav-item-dropdown>

                        <b-nav-item-dropdown text="Datos" rigth >
                            <b-dropdown-item to="/data/datos">Datos en general</b-dropdown-item>
                            <b-dropdown-item to="/data/boletin">Boletines</b-dropdown-item>
                            <b-dropdown-item to="/data/publicaciones">Publicaciones</b-dropdown-item>
                        </b-nav-item-dropdown>

                        <b-nav-item-dropdown text="Busqueda" rigth >
                            <b-dropdown-item to="/search/simple">Busqueda simple</b-dropdown-item>
                            <b-dropdown-item to="/search/functional_analysis">Anotacion funcional</b-dropdown-item>
                        </b-nav-item-dropdown>

                        <b-nav-item-dropdown text="Herramientas" rigth >
                            <b-dropdown-item to="/tools/blast">Blast+</b-dropdown-item>
                            <b-dropdown-item to="/tools/in_silico_pcr"><i>In silico</i> PCR</b-dropdown-item>
                            <b-dropdown-item to="/tools/seq_edit">Sequence Manipulation</b-dropdown-item>
                            <b-dropdown-item to="/tools/microsatellite">Simple Sequence Repeats</b-dropdown-item>
                            <b-dropdown-item to="/tools/external_tools">Herramienras externas</b-dropdown-item>

                        </b-nav-item-dropdown>
                        <b-nav-item to="/articulos">Notas</b-nav-item>
                        <b-nav-item to="/About">About</b-nav-item>
                        <b-nav-item to="/" disabled>Biotools</b-nav-item>
                    </b-navbar-nav>
               </b-collapse>
           </b-container>
       </b-navbar>
    </div>
</template>

<script>

export default {

    computed:{

    },

    methods: {

     }
}
</script>