<template>
    <div>
        <h1 class="text-center textocolor fredoka mt-5 pt-4">Crear Autómata</h1> 
        
        <div class="row justify-content-center">
            <div class="grafo1 col-md-3 mx-4  card cardaux">
                <div class="container-fluid mr-4">
                    <div class="row justify-content-center">
                        <div class="row my-3">
                            <div class="col-md-6"><button class="btn btn-success info textocolor">Crear autómata 1</button></div>
                            <div class="col-md-6"><button class="btn btn-success info textocolor">Crear autómata 2</button></div>
                        </div>
                    </div>   
                    <div class="my-3">
                        <h3 class="mt-3" >Autómata 1</h3>
                        <h3 class="mt-3" >Autómata 2</h3>
                        <h4 class="mt-3">Seleccione el tipo de autómata:</h4>
                        <select class="custom-select mb-3 mr-3 mt-2" v-model="option" >
                            <option selected :value="0">Seleccione un tipo de autómata</option>
                            <option :value="1">Autómata Finito Determinista (AFD)</option>
                            <option :value="2">Autómata Finito no Determinista (AFND)</option>
                        </select>
                        <button class="btn btn-sm btn-success">Volver</button>   
                    </div>         
                </div>
               
            </div>
            
        </div>
            

    </div>
</template>

<script>
export default {

    data(){
        return{
            
        }


    }

}
</script>