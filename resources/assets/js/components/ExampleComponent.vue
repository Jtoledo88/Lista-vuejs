<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel panel-default">
                    <div class="panel-heading">Example Component</div>

                    <div class="panel-body">
                        I'm an example component!
                        <input type="text" placeholder="Nombre" v-model="nombre">
                        <input type="text" placeholder="Apellido" v-model="apellido">
                        <h1>{{ nombre }}{{ apellido}}</h1>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        },
         data() {
             return {
                nombre: 'Jonathan',
                apellido: 'Toledo',
             }
         },
    }
</script>
