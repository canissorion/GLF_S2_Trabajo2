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
       <h1 class="text-center fredoka textocolor my-4">Operaciones</h1>
        <div class="row justify-content-center">
            <div class="card cardaux4 col-md-10 rounded-top">
                <div class="btn-group justify-content-center" role="group">
                    <a href="#afd-equivalente"><button class="btn btn-secondary" >AFD equivalente</button></a>
                    <a href="#complemento"><button class="btn btn-secondary" >Complemento</button></a>
                    <a href="#union"><button class="btn btn-secondary" >Unión</button></a>
                    <a href="#concatenacion"><button class="btn btn-secondary" >Concatenación</button></a>
                    <a href="#interseccion"><button class="btn btn-secondary" >Intersección</button></a>
                </div>
            </div>

            <!--Caso 1: AFD EQUIVALENTE -->
            <div class="cardaux2 col-md-10" >
                <h3 class="text-center fredoka textocolor my-3">AFD equivalente</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" >
                <div class="container my-3">
                    La conversión implica pasar por un AFD intermedio con estados y transiciones redundantes, luego se eliminan los estados inaccesibles o inalcanzables, es decir, aquellos a los que no se puede acceder a partir del estado inicial. Luego de este procedimiento, se obtiene el AFD equivalente.
                     <div class="my-3 text-center">
                        <button class="btn btn-success" >Mostrar AFD Equivalente</button>
                    </div>      
                    <div id="equivalente" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 2: COMPLEMENTO -->
            <div class="cardaux2 col-md-10" >
                <h3 class="text-center fredoka textocolor my-3">Complemento</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" >
                <div class="container my-3">
                    Esta propiedad es solo para AFD, intercambia los estados finales por no finales.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" >Mostrar Complemento Autómata 1</button>
                        <button class="btn btn-success" >Mostrar Complemento Autómata 2</button>
                    </div> 
                    <h4 class="text-center fredoka textocolor my-3" >Complemento Autómata 1</h4>   
                    <h4 class="text-center fredoka textocolor my-3" >Complemento Autómata 2</h4>
                    <div id="complemento" class="mb-3" style="border: 1px solid lightgray;"></div>
                    <div class="my-3 text-center" >
                        <h4 class="text-center fredoka textocolor my-3">AFD equivalente simplificado</h4>
                        <button class="btn btn-success" >Mostrar Autómata</button>
                        <div id="afdSimplificado" class="mb-3" style="border: 1px solid lightgray;"></div>
                    </div>

                </div>
            </div>

            <!--Caso 3: UNION -->
            <div class="cardaux2 col-md-10"  >
                <h3 class="text-center fredoka textocolor my-3">Unión</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" >
                <div class="container my-3">
                    La unión de dos lenguajes regulares es otro lenguaje regular, donde se utiliza la operación de unión de conjuntos, de manera que, Σ = {x,y} si L1 = {x,xy} y L2 = {yx,yy} la unión es L1UL2 = {x,xy,yx,yy}.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" >Mostrar unión</button>
                    </div>      
                    <div id="union" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 4: CONCATENACION -->
            <div class="cardaux2 col-md-10" >
                <h3 class="text-center fredoka textocolor my-3">Concatenación</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" >
                <div class="container my-3">
                    Se concatena una una cadena del primer lenguaje y una cadena del segundo lenguaje.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" >Mostrar concatenación</button>
                    </div>      
                    <div id="concatenacion" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 5: INTERSECCION -->
            <div class="cardaux2 col-md-10" >
                <h3 class="text-center fredoka textocolor my-3">Intersección</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" >
                <div class="container my-3">
                    La intersección de ambos lenguajes utiliza la operación de interseccion de conjuntos, Automata1 ∩ Automata2: 
                    <div class="my-3 text-center">
                        <button class="btn btn-success" >Mostrar Intersección</button>
                    </div>      
                    <div id="interseccion" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
                <div class="text-center">
                    <button class="btn btn-success" >SIMPLIFICA 1</button>
                    <div id="afdSimplificado" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
                <div class="text-center">
                    <button class="btn btn-success" >SIMPLIFICA 2</button>
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