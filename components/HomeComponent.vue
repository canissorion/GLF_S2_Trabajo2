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
                <div class="container-fluid py-4 mr-4">
                    <h3 class="mt-2">Autómata Finito Determinista (AFD)</h3>
                    <hr>
                    <div class="row text-center my-4">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success">Añadir Estado</button>
                        </div>
                        <div class="col-md-4">
                            <button type="button" class="btn btn-sm btn-success">Añadir Transición</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>
                    <div class="row text-center">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success">Modificar Estado Final</button>
                        </div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-danger">Eliminar Autómata</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>
                    
                    <div class="my-4">
                        <div>
                            <form >
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>
                            </form>       

                            <form>
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>

                            </form>
                        </div>
                    </div>
                    <div class="my-3">
                        <form>
                            <div class="form-group">
                                <label>Ingrese el id del estado inicial de la transición:</label>
                                <input type="number" min="0" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese el id del estado final de la transición:</label>
                                <input type="number" min="0" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese carácter de la transición: </label>
                                <input type="text" minlength="1" maxlength="1" class="form-control">
                            </div>
                    
                            <button class="btn btn-success btn-sm" type="submit" >Agregar</button>
                        </form>

                        <form>
                            <div class="form-group">
                                <label>Ingrese el id del estado inicial de la transición:</label>
                                <input type="number" min="0" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese el id del estado final de la transición:</label>
                                <input type="number" min="0" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese carácter de la transición: </label>
                                <input type="text" minlength="1" maxlength="1" class="form-control">
                            </div>
                    
                            <button class="btn  btn-success btn-sm" type="submit" >Agregar</button>
                        </form>
                    </div>

                    <div class="col-md-6 my-4">
                        <form>
                            <div class="form-group">
                                <label for="">Ingrese la palabra: </label>
                                <input type="text" class="form-control">
                            </div>
                         <button class="btn btn-sm btn-success" type="submit" >Consultar Palabra</button>
                        </form>
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