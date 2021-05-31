<template>
    <div class="container mt-3">
        <h2>{{this.$route.params.id}}</h2>
        <hr>
        <b-card>
            <b-text>
                <pre>{{secuencia}}</pre>
            </b-text>
        </b-card>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                secuencia : ''
            }
        },
        created () {
            this.getSecuencia();
        },
        methods: {
            async getSecuencia(){
                try {
                    const res = await this.$axios.get('/protein/getseq/'+this.$route.params.id)
                    this.secuencia = res.data.result
                } catch (error) {
                    console.log("ERROR:",error)
                }
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>