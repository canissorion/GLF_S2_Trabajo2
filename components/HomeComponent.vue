<template>
    <div>
        <!-- Tarjeta de crear automata:  -->
        <h1 class="text-center textocolor fredoka mt-5 pt-4">Crear Autómata</h1> 
        
        <div class="row justify-content-center">
            <div class="grafo1 col-md-3 mx-4  card cardaux" v-if="representacion1===false">
                <div class="container-fluid mr-4">
                    <div class="row justify-content-center" v-if="!automataCreate">
                        <div class="row my-3">
                            <div class="col-md-6"><button class="btn btn-success info textocolor" @click="selectAutomata1">Crear autómata 1</button></div>
                            <div class="col-md-6"><button class="btn btn-success info textocolor" @click="selectAutomata2">Crear autómata 2</button></div>
                        </div>
                    </div>   
                    <div v-if="automataCreate===true" class="my-3">
                        <h3 class="mt-3" v-if="selectAuto===1">Autómata 1</h3>
                        <h3 class="mt-3" v-if="selectAuto===2">Autómata 2</h3>
                        <h4 class="mt-3">Seleccione el tipo de autómata:</h4>
                        <select class="custom-select mb-3 mr-3 mt-2" v-model="option" >
                            <option selected :value="0">Seleccione un tipo de autómata</option>
                            <option :value="1">Autómata Finito Determinista (AFD)</option>
                            <option :value="2">Autómata Finito no Determinista (AFND)</option>
                        </select>
                        <button class="btn btn-sm btn-success" @click="back">Volver</button>   
                    </div>         
                </div>
                <div class="container-fluid py-4 mr-4" v-if="option===1">
                    <h3 class="mt-2">Autómata Finito Determinista (AFD)</h3>
                    <hr>
                    <div class="row text-center my-4">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success" @click="createEstado">Añadir Estado</button>
                        </div>
                        <div class="col-md-4">
                            <button type="button" class="btn btn-sm btn-success" @click="createTransicion">Añadir Transición</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>
                    <div class="row text-center">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success" @click="representacion">Modificar Estado Final</button>
                        </div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-danger" @click="delAndClear">Eliminar Autómata</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>
                    
                    <div class="my-4" v-if="validador">
                        <div>
                            <form @submit.prevent="agregarEstado" v-if="selectAuto===1">
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" v-model="estadoAutomata1.id" name="id" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>
                            </form>       

                            <form @submit.prevent="agregarEstado" v-else>
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" v-model="estadoAutomata2.id" name="id" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>

                            </form>
                        </div>
                    </div>
                    <div v-if="createTrans" class="my-3">
                        <form @submit.prevent="crearTransicion" v-if="selectAuto===1">
                            <div class="form-group">
                                <label>Ingrese el id del estado inicial de la transición:</label>
                                <input type="number" min="0" v-model="transicionAutomata1.from" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese el id del estado final de la transición:</label>
                                <input type="number" min="0" v-model="transicionAutomata1.to" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese carácter de la transición: </label>
                                <input type="text" minlength="1" maxlength="1"  v-model="transicionAutomata1.label" class="form-control">
                            </div>
                    
                            <button class="btn btn-success btn-sm" type="submit" >Agregar</button>
                        </form>

                        <form @submit.prevent="crearTransicion" v-else>
                            <div class="form-group">
                                <label>Ingrese el id del estado inicial de la transición:</label>
                                <input type="number" min="0" v-model="transicionAutomata2.from" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese el id del estado final de la transición:</label>
                                <input type="number" min="0" v-model="transicionAutomata2.to" class="form-control"> 
                            </div>
                            <div class="form-group">
                                <label>Ingrese carácter de la transición: </label>
                                <input type="text" minlength="1" maxlength="1" v-model="transicionAutomata2.label" class="form-control">
                            </div>
                    
                            <button class="btn  btn-success btn-sm" type="submit" >Agregar</button>
                        </form>
                    </div>

                    <div class="col-md-6 my-4">
                        <form @submit.prevent="consultarCadena">
                            <div class="form-group">
                                <label for="">Ingrese la palabra: </label>
                                <input type="text" class="form-control" v-model="cadena">
                            </div>
                         <button class="btn btn-sm btn-success" type="submit" >Consultar Palabra</button>
                        </form>
                    </div>


                </div>
                <div class="container-fluid py-4 mr-4" v-if="option===2">
                    <h3 class="mt-2">Autómata Finito no Determinista (AFND)</h3>
                    <hr>
                     <div class="row text-center my-4">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success" @click="createEstado">Añadir Estado</button>
                        </div>
                        <div class="col-md-4">
                            <button type="button" class="btn btn-sm btn-success" @click="createTransicion">Añadir Transición</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>
                    <div class="row text-center">
                        <div class="col-md-2"></div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-success" @click="representacion">Modificar Estado Final</button>
                        </div>
                        <div class="col-md-4">
                            <button class="btn btn-sm btn-danger" @click="delAndClear">Eliminar Autómata</button>
                        </div>
                        <div class="col-md-2"></div>
                    </div>

                   
                    <div class="my-4" v-if="validador">
                        <div>
                            <form @submit.prevent="agregarEstado" v-if="selectAuto===1">
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" v-model="estadoAutomata1.id" name="id" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>
                            </form>       

                            <form @submit.prevent="agregarEstado" v-if="selectAuto===2">
                                <div class="form-group">
                                    <label for="id">Ingrese el id: </label> 
                                    <input type="number" min="0" v-model="estadoAutomata2.id" name="id" class="form-control"> 
                                </div>
                                <div class="text-center">
                                    <button class="btn btn-success btn-sm" type="submit">Agregar</button>
                                </div>
                            </form>
                        </div>
                    </div>
                    <div v-if="createTrans" class="my-3">

                        <form @submit.prevent="crearTransicionAFND" v-if="selectAuto===1">
                                <div class="form-group">
                                    <label>Ingrese el id del estado inicial de la transición:</label>
                                    <input type="number" min="0" v-model="transicionAutomata1.from" class="form-control"> 
                                </div>
                                <div class="form-group">
                                    <label>Ingrese el id del estado final de la transición:</label>
                                    <input type="number" min="0" v-model="transicionAutomata1.to" class="form-control"> 
                                </div>
                                <div class="form-group">
                                    <label>Ingrese carácter de la transición: </label>
                                    <input type="text" minlength="1" maxlength="1"  v-model="transicionAutomata1.label" class="form-control">
                                </div>
                        
                                <button class="btn btn-success btn-sm" type="submit" >Agregar</button>
                        </form>

                        <form @submit.prevent="crearTransicionAFND" v-else>
                                <div class="form-group">
                                    <label>Ingrese el id del estado inicial de la transición:</label>
                                    <input type="number" min="0" v-model="transicionAutomata2.from" class="form-control"> 
                                </div>
                                <div class="form-group">
                                    <label>Ingrese el id del estado final de la transición:</label>
                                    <input type="number" min="0" v-model="transicionAutomata2.to" class="form-control"> 
                                </div>
                                <div class="form-group">
                                    <label>Ingrese carácter de la transición: </label>
                                    <input type="text" minlength="1" maxlength="1"  v-model="transicionAutomata2.label" class="form-control">
                                </div>
                        
                                <button class="btn btn-success btn-sm" type="submit" >Agregar</button>
                        </form>
                    </div>

                    <div class="col-md-6 my-4">
                        <form @submit.prevent="consultarCadena">
                            <div class="form-group">
                                <label for="">Ingrese la palabra: </label>
                                <input type="text" class="form-control" v-model="cadena">
                            </div>
                         <button class="btn btn-sm btn-success" type="submit">Consultar Palabra</button>
                        </form>
                    </div>
                </div>
            </div>
            <div class="grafo1 col-md-8  card cardaux">
                <h3 class="text-center fredoka my-2">Representación</h3>

                <div class="row">
                    <div class="col-md-6">
                        <h4 class="text-center fredoka my-3">Autómata 1</h4>
                        <div id="grafo" class="mb-3" style="border: 1px solid lightgray;"></div>
                    </div>
                    <div class="col-md-6">
                        <h4 class="text-center fredoka my-3">Autómata 2</h4>
                        <div id="grafo2" class="mb-3" style="border: 1px solid lightgray;"></div>
                    </div>
                </div>
                
            </div>
            <div class="grafo1 col-md-5 mx-3 card cardaux" v-if="representacion1===true">
                <hr>
                <div>
                    <table class="table table-striped table-dark" v-if="selectAuto===1" aria-describedby="estados1">
                        <thead>
                            <tr>
                            <th scope="col">#</th>
                            <th scope="col">Estado</th> 
                            <th scope="col">Final</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item,index) in estadosAutomata1" :key="index" v-show="index!=0">
                                <td scope="row">{{index}}</td>
                                <td>{{item.label}}</td>
                                <td>
                                    <div>             
                                        <input type="checkbox" name="state" id="state" @click="marcarFinal(item.id)" :checked="estadosAutomata1[index].final">
                                        <label for="state">Estado Final</label>
                                    </div>
                                </td>      
                            </tr>
                        </tbody>    
                    </table> 

                    <table class="table table-striped table-dark" v-if="selectAuto===2" aria-describedby="estados2">
                        <thead>
                            <tr>
                            <th scope="col">#</th>
                            <th scope="col">Estado</th> 
                            <th scope="col">Final</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item,index) in estadosAutomata2" :key="index" v-show="index!=0">
                                <td scope="row">{{index}}</td>
                                <td>{{item.label}}</td>
                                <td>
                                    <div>             
                                        <input type="checkbox" name="state" id="state" @click="marcarFinal(item.id)" :checked="estadosAutomata2[index].final">
                                        <label for="state">Estado Final</label>
                                    </div>
                                </td>      
                            </tr>
                        </tbody>    
                    </table> 
                    <button class="btn btn-success mb-3 btn-sm text-right" type="submit" @click="representacionBack">Guardar</button>
                </div>
            </div>
        </div>
            
        <h1 class="text-center fredoka textocolor my-4">Operaciones</h1>
        <div class="row justify-content-center">
            <div class="card cardaux4 col-md-10 rounded-top">
                <div class="btn-group justify-content-center" role="group">
                    <a href="#complemento"><button class="btn btn-secondary" @click="mostrarOp2">Complemento</button></a>
                    <a href="#union"><button class="btn btn-secondary" @click="mostrarOp3">Unión</button></a>
                    <a href="#concatenacion"><button class="btn btn-secondary" @click="mostrarOp4">Concatenación</button></a>
                    <a href="#interseccion"><button class="btn btn-secondary" @click="mostrarOp5">Intersección</button></a>
                </div>
            </div>

            <!--Caso 1: AFD EQUIVALENTE -->
            <div class="cardaux2 col-md-10" v-if="operacion===1">
                <h3 class="text-center fredoka textocolor my-3">AFD equivalente</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" v-if="operacion===1">
                <div class="container my-3">
                    La conversión implica pasar por un AFD intermedio con estados y transiciones redundantes, luego se eliminan los estados inaccesibles o inalcanzables, es decir, aquellos a los que no se puede acceder a partir del estado inicial. Luego de este procedimiento, se obtiene el AFD equivalente.
                     <div class="my-3 text-center">
                        <button class="btn btn-success" @click="drawAutomata">Mostrar AFD Equivalente</button>
                    </div>      
                    <div id="equivalente" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 2: COMPLEMENTO -->
            <div class="cardaux2 col-md-10" v-if="operacion===2">
                <h3 class="text-center fredoka textocolor my-3">Complemento</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" v-if="operacion===2">
                <div class="container my-3">
                    Esta propiedad es solo para AFD, intercambia los estados finales por no finales.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" @click="showComplemento(1)">Mostrar Complemento Autómata 1</button>
                        <button class="btn btn-success" @click="showComplemento(2)">Mostrar Complemento Autómata 2</button>
                    </div> 
                    <h4 class="text-center fredoka textocolor my-3" v-if="selectAuto===1">Complemento Autómata 1</h4>   
                    <h4 class="text-center fredoka textocolor my-3" v-else>Complemento Autómata 2</h4>
                    <div id="complemento" class="mb-3" style="border: 1px solid lightgray;"></div>
                    <div class="my-3 text-center" v-if="showSimplificado">
                        <h4 class="text-center fredoka textocolor my-3">AFD equivalente simplificado</h4>
                        <button class="btn btn-success" @click="mostrarSimplificado">Mostrar Autómata</button>
                        <div id="afdSimplificado" class="mb-3" style="border: 1px solid lightgray;"></div>
                    </div>

                </div>
            </div>

            <!--Caso 3: UNION -->
            <div class="cardaux2 col-md-10" v-if="operacion===3" >
                <h3 class="text-center fredoka textocolor my-3">Unión</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" v-if="operacion===3">
                <div class="container my-3">
                    La unión de dos lenguajes regulares es otro lenguaje regular, donde se utiliza la operación de unión de conjuntos, de manera que, Σ = {x,y} si L1 = {x,xy} y L2 = {yx,yy} la unión es L1UL2 = {x,xy,yx,yy}.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" @click="drawAutomata">Mostrar unión</button>
                    </div>      
                    <div id="union" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 4: CONCATENACION -->
            <div class="cardaux2 col-md-10" v-if="operacion===4">
                <h3 class="text-center fredoka textocolor my-3">Concatenación</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" v-if="operacion===4">
                <div class="container my-3">
                    Se concatena una una cadena del primer lenguaje y una cadena del segundo lenguaje.
                    <div class="my-3 text-center">
                        <button class="btn btn-success" @click="drawAutomata">Mostrar concatenación</button>
                    </div>      
                    <div id="concatenacion" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
            </div>

            <!--Caso 5: INTERSECCION -->
            <div class="cardaux2 col-md-10" v-if="operacion===5">
                <h3 class="text-center fredoka textocolor my-3">Intersección</h3>
            </div>
            <div id="afd-equivalente" class="card cardaux3 col-md-10 rounded-bottom mb-3" v-if="operacion===5">
                <div class="container my-3">
                    La intersección de ambos lenguajes utiliza la operación de interseccion de conjuntos, Automata1 ∩ Automata2: 
                    <div class="my-3 text-center">
                        <button class="btn btn-success" @click="drawAutomata">Mostrar Intersección</button>
                    </div>      
                    <div id="interseccion" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
                <div class="text-center">
                    <button class="btn btn-success" @click="simplificarAFD(estadosAutomata1,transicionesAutomata1)">SIMPLIFICA 1</button>
                    <div id="afdSimplificado" class="mb-3" style="border: 1px solid lightgray;"></div>
                </div>
                <div class="text-center">
                    <button class="btn btn-success" @click="simplificarAFD(estadosAutomata2,transicionesAutomata2)">SIMPLIFICA 2</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import vis from 'vis'

export default {

    data(){
        return{
            automata:{/*estados,alfabeto,transiciones,finales,inicio*/},

            estadosAutomata1:[{id:'inicio', label:'inicio',color:'#75616b47', final:false}],
            estadoAutomata1: {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'},
            transicionesAutomata1:[],
            transicionAutomata1:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            alfabeto1:[],

            estadosAutomata2:[{id:'inicio', label:'inicio',color:'#75616b47', final:false}],
            estadoAutomata2: {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'},
            transicionAutomata2:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesAutomata2:[],
            alfabeto2:[],
            
            
            automataUnion: [{id:'inicio', label:'inicio',color:'#75616b47', final:false}],
            transicionUnion:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesUnion:[],

            automataConcatenacion:[],
            estadoConcatenacion:{id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'},
            transicionConcatenacion:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesConcatenacion:[],

            automataComplemento:[],
            estadoComplemento:{id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'},
            transicionComplemento:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesComplemento:[],

            automataInterseccion:[{id:'inicio', label:'inicio',color:'#75616b47', final:false}],
            transicionInterseccion:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesInterseccion:[],
            
            automataEquivalente:[{id:'inicio', label:'inicio',color:'#75616b47', final:false}],
            transicionEquivalente:{from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}},
            transicionesEquivalente:[],

            automataSimplificado:[],
            transicionesSimplificado:[],
            transicionSimplificado:{from:'', label: '',to:'',color:{color:'rgb(0,0,0)'}},

            cadena:'',

            matrizAFD:[],
            matriz1AFND: [],
            option:0,
            operacion:'', 
            showSimplificado:false,
            validador:false,
            inicial:0,
            createTrans: false,
            representacion1:false,
            checked:false,
            automataCreate:false,
            selectAuto:'',
            dibujarSimplificado:false,

        }
    },
    

    created(){
        
    },

    methods:{
      
        selectAutomata1(){
            this.selectAuto=1
            this.automataCreate=true
            return
        },

        back(){
            this.automataCreate=false;
            this.option=0;
            this.createTrans=false;
            this.validador=false;
            return
        },
        
        selectAutomata2(){
            this.selectAuto=2;
            this.automataCreate=true;
            return
        },
        
        representacion(){
            this.representacion1=true;
            return;
        },
        representacionBack(){
            this.representacion1=false;
            return;
        },
        mostrarOp1(){

            return;
        },
        mostrarSimplificado(){
            this.dibujarSimplificado=true;
            this.drawAutomata();
        },

        

        showComplemento(opcion){
            if(opcion===1)
            {
                this.selectAuto=1;
                this.complemento();
                this.drawAutomata();
                this.showSimplificado=true;
            }
            else{
                this.selectAuto=2;
                this.complemento();
                this.drawAutomata();
            }
        },

        mostrarOp2(){
            this.dibujarSimplificado=false;
            if(this.existeFinales())
            {
                this.operacion=2;
                this.complemento();
                return;
            }
            else{
                this.$swal("Para proseguir debe marcar como final un estado final en los dos autómatas",{
                    className: "alertas",
                    button:'Aceptar',
                    title:"Aviso",
                });
                return;
            }
        },

        mostrarOp3(){
            this.dibujarSimplificado=false;
            if(this.existeFinales())
            {
                this.operacion=3;
                this.union();
                this.crearMatrizTransiciones(this.transicionesAutomata2);
                this.crearMatrizTransiciones(this.transicionesAutomata1);
                return;
            }
            else{
                this.$swal("Para proseguir debe marcar como final un estado final en los dos autómatas",{
                    className: "alertas",
                    button:'Aceptar',
                    title:"Aviso",
                });
                return;
            }
        },

        mostrarOp4(){
            this.dibujarSimplificado=false;
            if(this.existeFinales())
            {
                this.operacion=4;
                this.concatenacion();
                return;
            }
            else{
                this.$swal("Para proseguir debe marcar como final un estado final en los dos autómatas",{
                    className: "alertas",
                    button:'Aceptar',
                    title:"Aviso",
                });
                return;
            }
        },

        mostrarOp5(){
            this.dibujarSimplificado=false;
            if(this.existeFinales())
            {
                this.automataSimplificado=[];
                this.transicionesSimplificado=[];
                this.operacion=5;
                this.interseccion();
                return;
            }
            else{
                this.$swal("Para proseguir debe marcar como final un estado final en los dos autómatas",{
                    className: "alertas",
                    button:'Aceptar',
                    title:"Aviso",
                });
                return;
            }
        },

        marcarInicial(){
            this.inicial=1;
            return;
        },
        
        consultarCadena(){
            var word=this.cadena.split('');
            var transicionesEstadoActual=[];
            var estadoActual;
            console.log(word);
            if(this.selectAuto===1)
            {
                if(this.estadosAutomata1.length===1 || this.estadosAutomata1.length===0)
                {
                    this.$swal("Debe ingresar el autómata antes de analizar la palabra",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                else{
                    if(!this.existeFinal(this.estadosAutomata1))
                    {
                        this.$swal("Para analizar la palabra debe marcar como final un estado final en el autómata seleccionado",{
                            className: "alertas",
                            button:'Aceptar',
                            title:"Aviso"
                        });
                        return;
                    }
                }
                estadoActual=this.estadosAutomata1[1].id;
                console.log(estadoActual);
                for(var i=0;i<word.length;i++)
                {
                    if(!this.existeCaracter(word[i]))
                    {
                        this.$swal("La palabra no pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                        });
                        return;
                    }
                    //llena con las transiciones que corresponden del estado actual
                    for(var j=0; j<this.transicionesAutomata1.length;j++)
                    {
                        if(this.transicionesAutomata1[j].from==estadoActual)
                        {
                            transicionesEstadoActual.push(this.transicionesAutomata1[j]);
                        }
                    }
                    //recorre las transiciones del estado actual para pasar al otro de acuerdo al caracter actual
                    for(var k=0; k<transicionesEstadoActual.length;k++)
                    {
                        if(transicionesEstadoActual[k].label==word[i])
                        {
                            estadoActual=transicionesEstadoActual[k].to;
                            console.log('estado actual: ', estadoActual);
                        }
                    }
                    transicionesEstadoActual=[];
                }

                for(var l=0; l<this.estadosAutomata1.length;l++)
                {
                    if(estadoActual===this.estadosAutomata1[l].id)
                    {
                        if(this.estadosAutomata1[l].final==true)
                        {
                            this.$swal("La palabra pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                            });
                            return;
                        }
                        else
                        {
                            this.$swal("La palabra no pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                            });
                            return;
                        }  
                    }
                }                        
            }  
            else{
                if(this.estadosAutomata2.length===1 || this.estadosAutomata2.length===0)
                {
                    this.$swal("Debe ingresar el autómata antes de analizar la palabra",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                else{
                    if(!this.existeFinal(this.estadosAutomata2))
                    {
                        this.$swal("Para analizar la palabra debe marcar como final un estado final en el autómata seleccionado",{
                            className: "alertas",
                            button:'Aceptar',
                            title:"Aviso"
                        });
                        return;
                    }
                }
                estadoActual=this.estadosAutomata2[1].id;
                console.log(estadoActual);
                for(var i=0;i<word.length;i++)
                {
                    if(!this.existeCaracter(word[i]))
                    {
                        this.$swal("La palabra no pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                        });
                        return;
                    }
                    //llena con las transiciones que corresponden del estado actual
                    for(var j=0; j<this.transicionesAutomata2.length;j++)
                    {
                        if(this.transicionesAutomata2[j].from==estadoActual)
                        {
                            transicionesEstadoActual.push(this.transicionesAutomata2[j]);
                        }
                    }
                    //recorre las transiciones del estado actual para pasar al otro de acuerdo al caracter actual
                    for(var k=0; k<transicionesEstadoActual.length;k++)
                    {
                        if(transicionesEstadoActual[k].label==word[i])
                        {
                            estadoActual=transicionesEstadoActual[k].to;
                            console.log('estado actual: ', estadoActual);
                        }
                    }
                    transicionesEstadoActual=[];
                }

                for(var l=0; l<this.estadosAutomata2.length;l++)
                {
                    if(estadoActual===this.estadosAutomata2[l].id)
                    {
                        if(this.estadosAutomata2[l].final==true)
                        {
                            this.$swal("La palabra  pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                            });
                            return
                        }
                        else
                        {
                            this.$swal("La palabra no pertenence al lenguaje",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Análisis de palabra"
                            });
                            return;
                        }  
                    }
                } 
            }    
        },

        existeCaracter(caracter){
            if(this.selectAuto===1)
            {
                for(var i=0;i<this.alfabeto1.length;i++)
                {
                    if(this.alfabeto1[i]==caracter)
                    {
                        return true
                    }
                }
                return false;
            }
            else{
                for(var i=0;i<this.alfabeto2.length;i++)
                {
                    if(this.alfabeto2[i]==caracter)
                    {
                        return true
                    }
                }
                return false;

            }
        },

        estadosDistintos(estados1,estado){
            for(var i=0; i<estados1.length; i++)
            {
                if(estados1[i].id===estado.id)
                {
                    
                    return false;
                }
            }
            return true;
        },

        agregarEstado(){
            var aux= {id:'inicio', label:'inicio',color:'#75616b47', final:false}

            if(this.selectAuto===1)
            {
                if(this.estadoAutomata1.id==='')
                {
                    this.$swal("Por favor, ingrese un id.",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                if(this.existeEstado(this.estadosAutomata1,this.estadoAutomata1))
                {
                    this.$swal("El estado ya existe. Ingrese un estado con otro id.",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                if(this.estadosAutomata1.length===0)
                {
                    this.estadosAutomata1.push(aux);
                }
                this.estadoAutomata1.label=this.estadoAutomata1.id;
                if(this.estadosAutomata1.length===1)
                {
                    if(!this.estadosDistintos(this.estadosAutomata2,this.estadoAutomata1))
                    {
                        this.$swal("El id debe ser distinto al/los estado(s) del otro automata",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Alerta"
                        });
                        return;
                    }
                    this.estadosAutomata1.push(this.estadoAutomata1);
                    this.transicionAutomata1.from='inicio';
                    this.transicionAutomata1.to=this.estadoAutomata1.id;
                    this.transicionAutomata1.label='';
                    this.transicionesAutomata1.push(this.transicionAutomata1);
                    this.transicionAutomata1={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                    this.estadoAutomata1= {id:'', label:'',color:'#C52C0B' ,final:false};
                    this.drawAutomata();
                    return;
                }
                else{
                    if(!this.estadosDistintos(this.estadosAutomata2,this.estadoAutomata1))
                    {
                        this.$swal("El id debe ser distinto al/los estado(s) del otro automata",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Alerta"
                        });
                        return;
                    }
                    this.estadosAutomata1.push(this.estadoAutomata1);
                    this.estadoAutomata1= {id:'', label:'',color:'#C52C0B' ,final:false};
                    this.drawAutomata();
                }

            }
            else{
                if(this.estadoAutomata2.id==='') 
                {
                    this.$swal("Por favor, ingrese un id.",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                if(this.existeEstado(this.estadosAutomata2,this.estadoAutomata2))
                {
                    this.$swal("El estado ya existe. Ingrese un estado con otro id.",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                if(this.estadosAutomata2.length===0)
                {
                    this.estadosAutomata2.push(aux);
                }
                this.estadoAutomata2.label=this.estadoAutomata2.id;
                if(this.estadosAutomata2.length===1)
                {
                    if(!this.estadosDistintos(this.estadosAutomata1,this.estadoAutomata2))
                    {
                        this.$swal("El id debe ser distinto al/los estado(s) del otro automata",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Alerta"
                        });
                        return;
                    }
                    this.estadosAutomata2.push(this.estadoAutomata2);
                    this.transicionAutomata2.from='inicio';
                    this.transicionAutomata2.to=this.estadoAutomata2.id;
                    this.transicionAutomata2.label='';
                    this.transicionesAutomata2.push(this.transicionAutomata2);
                    this.transicionAutomata2={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                    this.estadoAutomata2= {id:'', label:'',color:'#C52C0B' ,final:false};
                    this.drawAutomata();
                    return;
                }
                else{
                    if(!this.estadosDistintos(this.estadosAutomata1,this.estadoAutomata2))
                    {
                        this.$swal("El id debe ser distinto al/los estado(s) del otro automata",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Alerta"
                        });
                        return;
                    }
                    this.estadosAutomata2.push(this.estadoAutomata2);
                    this.estadoAutomata2= {id:'', label:'',color:'#C52C0B' ,final:false};
                    this.drawAutomata();
                    return;
                }
            }
            
            
        },

        existeEstado(estados,estado)
        {
            for(var i=0; i<estados.length;i++)
            {
                if(estados[i].id===estado.id)
                {
                    return true;
                }
            }
            return false;
        },

        existeFinal(estados){
            var existe= false;
            for(var i=0; i<estados.length;i++)
            {
                if(estados[i].final===true)
                {
                    existe=true;
                }
                else{
                    if(estados[i].final===false && existe===true)
                    {
                        existe=true;
                    }
                    else{
                        if(estados[i].final===false && existe===false)
                        {
                            existe=false;
                        }
                    }
                }
            }
            if(existe)
            {
                return true;
            }
            return false;
        },

        existeFinales(){
            var existe1=false;
            var existe2=false;

            for(var i=0; i<this.estadosAutomata1.length;i++)
            {
                if(this.estadosAutomata1[i].final===true)
                {
                    existe1=true;
                }
                else{
                    if(this.estadosAutomata1[i].final===false && existe1===true)
                    {
                        existe1=true;
                    }
                    else{
                        if(this.estadosAutomata1[i].final===false && existe1===false)
                        {
                            existe1=false;
                        }
                    }
                }
            }

            for(var j=0; j<this.estadosAutomata2.length;j++)
            {
                if(this.estadosAutomata2[j].final===true)
                {
                    existe2=true;
                }
                else{
                    if(this.estadosAutomata2[j].final===false && existe2===true)
                    {
                        existe2=true;
                    }
                    else{
                        if(this.estadosAutomata2[j].final===false && existe2===false)
                        {
                            existe2=false;
                        }
                    }
                }
            }

            if(existe1 && existe2)
            {
                return true;
            }
            return false;
        },

        createEstado(){

            if(this.selectAuto===1)
            {
                console.log("estado",this.estadoAutomata1);
                console.log("estados",this.estadosAutomata1);
                console.log("transicion",this.transicionAutomata1);
                console.log("transiciones",this.transicionesAutomata1);

            }
            else{
                console.log("estado",this.estadoAutomata2);
                console.log("estados",this.estadosAutomata2);
                console.log("transicion",this.transicionAutomata2);
                console.log("transiciones",this.transicionesAutomata2);
            }           
            this.validador=true;
            this.createTrans=false;
        },


        createTransicion(){
            this.createTrans=true;
            this.validador=false;
        },

        delAndClear(){  

            if(this.selectAuto===1)
            {
                this.estadosAutomata1=[]
                this.transicionesAutomata1=[]

            }
            else{
                this.estadosAutomata2=[]
                this.transicionesAutomata2=[]   
            }
               
            console.log("Automata Eliminado");
            this.drawAutomata();
        },

        marcarFinal(id)
        {
            if(this.selectAuto===1)
            {
                  for(var i=0; i<this.estadosAutomata1.length;i++){
                    if(this.estadosAutomata1[i].id==id  && this.estadosAutomata1[i].final==false){
                        this.estadosAutomata1[i].final=true;
                        this.estadosAutomata1[i].shape='diamond';
                        this.estadosAutomata1[i].color='#5cb85c';
                        this.drawAutomata();
                        console.log("final",i ,"",this.estadosAutomata1[i].final);
                    }
                    else
                    {
                        if(this.estadosAutomata1[i].id==id  && this.estadosAutomata1[i].final==true){
                            this.estadosAutomata1[i].final=false;
                            this.estadosAutomata1[i].shape='ellipse';
                            this.estadosAutomata1[i].color='#C52C0B';
                            this.drawAutomata();
                        }
                    }
                    console.log("estado: ",this.estadosAutomata1[i].label, this.estadosAutomata1[i].final);
                } 

            }
            else{
                  for(var i=0; i<this.estadosAutomata2.length;i++){
                    if(this.estadosAutomata2[i].id==id  && this.estadosAutomata2[i].final==false){
                        this.estadosAutomata2[i].final=true;
                        this.estadosAutomata2[i].shape='diamond';
                        this.estadosAutomata2[i].color='#5cb85c';
                        this.drawAutomata();
                        console.log("final",i ,"",this.estadosAutomata2[i].final);
                    }
                    else
                    {
                        if(this.estadosAutomata2[i].id==id  && this.estadosAutomata2[i].final==true){
                            this.estadosAutomata2[i].final=false;
                            this.estadosAutomata2[i].shape='ellipse';
                            this.estadosAutomata2[i].color='#C52C0B';
                            this.drawAutomata();
                        }
                    }
                    console.log("estado: ",this.estadosAutomata2[i].label, this.estadosAutomata2[i].final);
                } 
            }
            
        },
 
        crearTransicion(){
            
            if(this.selectAuto===1){
                if(this.transicionAutomata1.from==='' || this.transicionAutomata1.to==='' )
                {
                    this.$swal("Estados o caracter no ingresados . Rellene todos los campos antes de continuar",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                
                for(var i=0; i<this.transicionesAutomata1.length;i++)
                {
                    if(this.transicionesAutomata1[i].from===this.transicionAutomata1.from && this.transicionesAutomata1[i].label===this.transicionAutomata1.label)
                    {
                        this.$swal("la transición ya existe. Ingrese otra",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    
                }
                if(!this.existeEstadoTransicion(this.estadosAutomata1,this.transicionAutomata1))
                {
                    this.$swal("Los estados ingresados no existen, Ingrese otros estados para generar la transición",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                this.addCaracterToAlfabeto();
                this.transicionesAutomata1.push(this.transicionAutomata1);
                this.transicionAutomata1={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            }
            else{
                if(this.transicionAutomata2.from==='' || this.transicionAutomata2.to==='' )
                {
                    this.$swal("Estados o caracter no ingresados . Rellene todos los campos antes de continuar",{
                        className: "alertas",
                        buton: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                
                for(var i=0; i<this.transicionesAutomata2.length;i++)
                {
                    if(this.transicionesAutomata2[i].from===this.transicionAutomata2.from && this.transicionesAutomata2[i].label===this.transicionAutomata2.label)
                    {
                        this.$swal("la transición ya existe. Ingrese otra",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    
                }
                if(!this.existeEstadoTransicion(this.estadosAutomata2,this.transicionAutomata2))
                {
                    this.$swal("Los estados ingresados no existen, Ingrese otros estados para generar la transición",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                this.addCaracterToAlfabeto();
                this.transicionesAutomata2.push(this.transicionAutomata2);
                this.transicionAutomata2={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};

            }
            this.drawAutomata();          
        },

        crearTransicionAFND(){           
            if(this.selectAuto===1){
                if(this.transicionAutomata1.from==='' || this.transicionAutomata1.to==='' )
                {
                    this.$swal("Estados o caracter no ingresados . Rellene todos los campos antes de continuar",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }
                if(this.existeTransicion(this.transicionesAutomata1,this.transicionAutomata1))
                {
                   this.$swal("la transición ya existe. Ingrese otra",{
                       className: "alertas",
                       button: "Aceptar",
                       title: "Error"
                   });
                   return;
                }
                else{
                    if(!this.existeEstadoTransicion(this.estadosAutomata1,this.transicionAutomata1))
                    {
                        this.$swal("Los estados ingresados no existen, Ingrese otros estados para generar la transición",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    if(this.existeTransicionAFND(this.transicionesAutomata1,this.transicionAutomata1))
                    {
                        this.$swal("La transicion ya existe. Ingrese otra",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    for(var i=0; i<this.transicionesAutomata1.length;i++)
                    {
                        if(this.transicionesAutomata1[i].from===this.transicionAutomata1.from && this.transicionesAutomata1[i].label!=this.transicionAutomata1.label && this.transicionesAutomata1[i].to===this.transicionAutomata1.to){
                            var aux = [this.transicionesAutomata1[i].label, this.transicionAutomata1.label]
                            console.log("aux:", aux);
                            this.transicionesAutomata1[i].label= aux[0]+','+aux[1];
                            console.log("label",this.transicionesAutomata1[i].label);
                            this.drawAutomata();
                            this.addCaracterToAlfabeto();
                            return;    
                        }                    
                    }
                }               
                this.addCaracterToAlfabeto();
                this.transicionesAutomata1.push(this.transicionAutomata1);
                this.transicionAutomata1={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            }
            else{
                if(this.transicionAutomata2.from==='' || this.transicionAutomata2.to==='' )
                {
                    this.$swal("Estados o caracter no ingresados . Rellene todos los campos antes de continuar",{
                        className: "alertas",
                        button: "Aceptar",
                        title: "Error"
                    });
                    return;
                }

                if(this.existeTransicion(this.transicionesAutomata2,this.transicionAutomata2))
                {
                   this.$swal("la transición ya existe. Ingrese otra",{
                       className: "alertas",
                       button: "Aceptar",
                       title: "Error"
                   });
                   return;
                }
                else{
                    if(!this.existeEstadoTransicion(this.estadosAutomata2,this.transicionAutomata2))
                    {
                        this.$swal("Los estados ingresados no existen, Ingrese otros estados para generar la transición",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    if(this.existeTransicionAFND(this.transicionesAutomata2,this.transicionAutomata2))
                    {
                        this.$swal("La transicion ya existe. Ingrese otra",{
                            className: "alertas",
                            button: "Aceptar",
                            title: "Error"
                        });
                        return;
                    }
                    for(var i=0; i<this.transicionesAutomata2.length;i++)
                    {
                        if(this.transicionesAutomata2[i].from===this.transicionAutomata2.from && this.transicionesAutomata2[i].label!=this.transicionAutomata2.label && this.transicionesAutomata2[i].to===this.transicionAutomata2.to)
                        {
                            var aux = [this.transicionesAutomata2[i].label, this.transicionAutomata2.label]
                            console.log("aux:", aux);
                            this.transicionesAutomata2[i].label= aux[0]+','+aux[1];
                            console.log("label",this.transicionesAutomata2[i].label);
                            this.drawAutomata();
                            this.addCaracterToAlfabeto();
                            return;   
                        }        
                    }     
                }          
                this.addCaracterToAlfabeto();
                this.transicionesAutomata2.push(this.transicionAutomata2);
                this.transicionAutomata2={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            }
            this.drawAutomata(); 
        },

        existeTransicionAFND(transiciones,transicion){
            var existe=false;
            var caracteres=[];
            var aux;
            for(var i=0; i<transiciones.length;i++)
            {
                if(transiciones[i].from===transicion.from && transiciones[i].to===transicion.to)
                {
                    if(transiciones[i].label.includes(','))
                    {
    
                        aux=transiciones[i].label;
                        caracteres=aux.split(',');
                        for(var j=0; j<caracteres.length;j++)
                        {
                            if(caracteres[j]===transicion.label)
                            {
                                return true;
                            }
                        }
                        return false;
                        
                    }
                }
            }
            
        },

        existeTransicion(transiciones,transicion)
        {
            for(var i=0; i<transiciones.length;i++)
            {
                if(transiciones[i].from===transicion.from && transiciones[i].to === transicion.to && transiciones[i].label===transicion.label)
                {
                    return true;
                }
            }
            return false;

        },

        existeEstadoTransicion(estados, transicion)
        {
            var existeFrom=false;
            var existeTo= false;
            for(var i=0; i<estados.length;i++)
            {
                if(estados[i].id===transicion.from)
                {
                    existeFrom=true;
                }
                else{
                    if(estados[i].id!=transicion.from && existeFrom===true)
                    {
                        existeFrom=true;
                    }
                    else{
                        if(estados[i].id!=transicion.from && existeFrom===false)
                        {
                            existeFrom=false;
                        }
                    }
                }
            }
            for(var j=0; j<estados.length;j++)
            {
                if(estados[j].id===transicion.to)
                {
                    existeTo=true;
                }
                else{
                    if(estados[j].id!=transicion.to && existeTo===true)
                    {
                        existeTo=true;
                    }
                    else{
                        if(estados[j].id!=transicion.to && existeTo===false)
                        {
                            existeTo=false;
                        }
                    }
                }
            }

            if(existeFrom && existeTo)
            {
                return true;
            }
            else{
                return false;
            }
        },


        addCaracterToAlfabeto(){

            var existe=false;

            if(this.selectAuto===1)
            {
                for (var i=0; i<this.transicionesAutomata1.length;i++)
                {
                    if(existe===true && this.transicionAutomata1.label!= this.transicionesAutomata1[i].label)
                    {
                        existe=true
                    }
                    else{
                        if(this.transicionesAutomata1[i].label===this.transicionAutomata1.label)
                        {
                            existe=true;
                        }
                        else{
                            existe=false;
                        }
                    }
                }
                
                if(!existe)
                {
                    this.alfabeto1.push(this.transicionAutomata1.label);
                }

                console.log("alfabeto1",this.alfabeto1);
            }
            else{
                for (var i=0; i<this.transicionesAutomata2.length;i++) // a, b, b, a
                {
                    
                    if(existe===true && this.transicionAutomata2.label!= this.transicionesAutomata2[i].label)
                    {
                        existe=true
                    }
                    else{
                        if(this.transicionesAutomata2[i].label===this.transicionAutomata2.label)
                        {
                            existe=true;
                        }
                        else{
                            existe=false;
                        }
                    }
                }
                
                if(!existe)
                {
                    this.alfabeto2.push(this.transicionAutomata2.label);
                }
                

                console.log("alfabeto2: ",this.alfabeto2);
            }
            
        },
        

        crearMatrizTransiciones(transiciones){
            var res = [];
            for(var i=0; i<3;i++){
                res[i]= new Array(transiciones.length);
            }
            for(var t=1; t<transiciones.length;t++){
                res[0][t]=transiciones[t].from
                res[1][t]=transiciones[t].label
                res[2][t]=transiciones[t].to
            }
        
            console.log("Matriz ",res);
            return res;
        },

        encontrarComa(data)
        {
            var cadena= data;
            if(cadena.includes(','))
            {
                return true;
            }
            else{
                return false;
            }
        },
        matrizTransicionesUnion()
        {
            console.log("funcion matriz transiciones union");
            var matAuto1= this.crearMatrizTransiciones(this.transicionesAutomata1);
            var matAuto2= this.crearMatrizTransiciones(this.transicionesAutomata2); 
            console.log("matAuto1", matAuto1);
            console.log("matAuto2", matAuto2);
            var strId;
            var matrix=[];
            var union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            var transicion, transicion2;
            var to1, to2;
            console.log("automata union", this.automataUnion);
            for(let i=1;i<this.automataUnion.length; i++){
                strId = this.automataUnion[i].id.split(','); 
                console.log("id automata union",this.automataUnion[i].id);
                for(var j=1; j<matAuto1[0].length;j++)
                {   
                    console.log("Compara", strId[0]," con ",matAuto1[0][j]);
                    if(strId[0]===matAuto1[0][j])
                    {
                        console.log("strId", strId[0], strId[1]);
                        transicion=matAuto1[1][j];
                        console.log("transicion",transicion);
                        to1=matAuto1[2][j];
                        if (this.encontrarComa(transicion)){ 
                            var arraytransicion= transicion.split(',');
                            for(var m=0; m<arraytransicion.length; m++){
                                transicion= arraytransicion[m];
                                for(var k=1; k<matAuto2[2].length;k++){
                                    transicion2=matAuto2[1][k];
                                    console.log("transicion",transicion2);
                                    if(this.encontrarComa(transicion2)){   //a,b - a,b
                                        var arraytransicion2= transicion2.split(',');
                                        for(var o=0; o<arraytransicion2.length; o++){
                                            transicion2= arraytransicion2[o];
                                            if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                                to2=matAuto2[2][k];
                                                union.from=strId[0]+','+strId[1];
                                                union.label=transicion;
                                                union.to=to1+','+to2;
                                                console.log("union 1 from",union.from);
                                                console.log("union 1 transicion",union.label);
                                                console.log("union 1 to",union.to);
                                                matrix.push(union);
                                                union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                            }
                                        }
                                    }else{                              //a,b - a
                                        if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                            to2=matAuto2[2][k];
                                            union.from=strId[0]+','+strId[1];
                                            union.label=transicion;
                                            union.to=to1+','+to2;
                                            console.log("union 2 from",union.from);
                                            console.log("union 2 transicion",union.label);
                                            console.log("union 2 to",union.to);
                                            matrix.push(union);
                                            union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                        }
                                    }
                                    transicion2=null;
                                }
                                transicion=null;
                            }
                            
                        }else{ //a - a, b      y a - a
                            for(var k=1; k<matAuto2[2].length;k++){
                                transicion2=matAuto2[1][k];
                                console.log("transicion",transicion2);
                                if(this.encontrarComa(transicion2)){   //a - a,b
                                    var arraytransicion2= transicion2.split(',');
                                    for(var o=0; o<arraytransicion2.length; o++){
                                        transicion2= arraytransicion2[o];
                                        if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                            to2=matAuto2[2][k];
                                            union.from=strId[0]+','+strId[1];
                                            union.label=transicion;
                                            union.to=to1+','+to2;
                                            console.log("union 3 from",union.from);
                                            console.log("union 3 transicion",union.label);
                                            console.log("union 3 to",union.to);
                                            matrix.push(union);
                                            union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                        }
                                    }
                                }else{                              //a - a
                                    if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                        to2=matAuto2[2][k];
                                        union.from=strId[0]+','+strId[1];
                                        union.label=transicion;
                                        union.to=to1+','+to2;
                                        console.log("union 4 from",union.from);
                                        console.log("union 4 transicion",union.label);
                                        console.log("union 4 to",union.to);
                                        matrix.push(union);
                                        union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                    }
                                }
                                transicion2=null;
                            }
                        }
                    }
                }           
            }
            console.log("matrix", matrix);
            this.transicionesUnion=matrix;
        },
        

        compararAlfabetos()
        {
            var cont=0;
            for(var i=0; i< this.alfabeto1.length; i++)
            {
                for(var j=0; j< this.alfabeto2.length; j++)
                {
                    if(this.alfabeto1[i]==this.alfabeto2[j])
                    {
                        cont++;
                    }
                }
            }
            if(cont== this.alfabeto1.length && cont== this.alfabeto2.length)
            {
                return true;
            }
            return false;
        },

        union(){
            if(!this.compararAlfabetos())
            {
                this.$swal("Los alfabetos de ambos automatas deben ser iguales",{
                    className: "alertas",
                    button: "Aceptar",
                    title:"Error"
                });
                return;
            }
            this.automataUnion= [{id:'inicio', label:'inicio',color:'#75616b47', final:false}];
            this.transicionUnion= {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            this.transicionesUnion= [];
            var auto1= this.transicionesAutomata1
            var auto2 =this.transicionesAutomata2
            console.log("alfabeto 1", this.alfabeto1);
            console.log("alfabeto 2", this.alfabeto2);
            var autoUnion = '';
            var autoaux = {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'}
            this.alfabeto1=this.simplificarAlfabeto(this.alfabeto1);
            this.alfabeto2=this.simplificarAlfabeto(this.alfabeto2);
            if(this.compararAlfabetos()){
                var nuevoAlfabeto = this.alfabeto1;
                for(var i=1 ; i<this.estadosAutomata1.length ;i++){
                    for(var k=1; k<this.estadosAutomata2.length; k++){
                        autoaux.label=this.estadosAutomata1[i].label+this.estadosAutomata2[k].label;
                        autoaux.id=this.estadosAutomata1[i].label+','+this.estadosAutomata2[k].label;
                        if(this.estadosAutomata1[i].final==true || this.estadosAutomata2[k].final==true){
                            autoaux.final=true;
                            autoaux.shape='diamond';
                            autoaux.color='#5cb85c';
                        }
                        this.automataUnion.push(autoaux);
                        console.log(this.automataUnion);  
                        
                        autoaux = {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'};
                    }
                }        
                this.matrizTransicionesUnion();
                this.transicionUnion.from='inicio';
                this.transicionUnion.label='';
                this.transicionUnion.to=this.automataUnion[1].id;
                this.transicionesUnion.push(this.transicionUnion);
                console.log(this.transicionesUnion);
            }
            else{
                var nuevoAlfabeto = this.alfabeto1.concat(this.alfabeto2);
                var purgado= this.simplificarAlfabeto(nuevoAlfabeto);
                nuevoAlfabeto=purgado;
                console.log("nuevo Alfabeto:",nuevoAlfabeto);
                for(var i=1; i<this.estadosAutomata1.length; i++)
                {
                    for(var j=1;j<this.estadosAutomata2.length;j++)
                    {
                        
                    }
                }
            }
        },

        simplificarAlfabeto(alfabeto){
            
            var newArr = [];
            var myObj = {};
            alfabeto.forEach(el => {      
                if (!(el in myObj)) { 
                    myObj[el] = true
                    newArr.push(el)
                }
            });
                        
            console.log(newArr);
            return newArr;
        },
        
        drawAutomata(){
            var container= document.getElementById("grafo");
            var container2= document.getElementById("grafo2");
            var containerUnion= document.getElementById("union");
            var containerInterseccion= document.getElementById("interseccion");
            var containerComplemento= document.getElementById("complemento");
            var containerConcatenacion= document.getElementById("concatenacion");
            var containerEquivalente= document.getElementById("equivalente");
            var containerSimplificado= document.getElementById("afdSimplificado");

            var data={nodes:this.estadosAutomata1,
                      edges:this.transicionesAutomata1};

            var data2={nodes:this.estadosAutomata2,
                       edges:this.transicionesAutomata2};
            
            var dataUnion={nodes:this.automataUnion,
                           edges:this.transicionesUnion};
            
            var dataInterseccion={nodes:this.automataInterseccion,
                                  edges:this.transicionesInterseccion};
            
            var dataComplemento={nodes:this.automataComplemento,
                                 edges:this.transicionesComplemento};

            var dataConcatenacion={nodes:this.automataConcatenacion,
                                   edges:this.transicionesConcatenacion};
            
            var dataEquivalente={nodes:this.automataEquivalente,
                                 edges:this.transicionesEquivalente};
            
            var dataSimplificado={nodes:this.automataSimplificado,
                                  edges:this.transicionesSimplificado};

            var options = {
                height: 320 + 'px',
                
                edges:{
                    
                    arrows:'to',
                },
            };

    

            var network= new vis.Network(container,data,options);
            var network2= new vis.Network(container2,data2,options);

            if(this.operacion===1)
            {
                var networkEquivalente= new vis.Network(containerEquivalente,dataEquivalente,options);
                if(this.dibujarSimplificado===true)
                {
                    var networkSimplificado= new vis.Network(containerSimplificado,dataSimplificado,options);
                }
            }
            if(this.operacion===2)
            {
                var networkComplemento= new vis.Network(containerComplemento,dataComplemento,options);
                if(this.dibujarSimplificado===true)
                {
                    var networkSimplificado= new vis.Network(containerSimplificado,dataSimplificado,options);
                }
                
            }
            if(this.operacion===3)
            {
                var networkUnion= new vis.Network(containerUnion,dataUnion,options);
                if(this.dibujarSimplificado===true)
                {
                    var networkSimplificado= new vis.Network(containerSimplificado,dataSimplificado,options);
                }
            }
            if(this.operacion===4)
            {
                var networkConcatenacion= new vis.Network(containerConcatenacion,dataConcatenacion,options);
                if(this.dibujarSimplificado===true)
                {
                    var networkSimplificado= new vis.Network(containerSimplificado,dataSimplificado,options);
                }
            }
            if(this.operacion===5)
            {
                var networkInterseccion= new vis.Network(containerInterseccion,dataInterseccion,options);
                if(this.dibujarSimplificado===true)
                {
                    var networkSimplificado= new vis.Network(containerSimplificado,dataSimplificado,options);
                }
            }
            
        },

        matrizAFND(){
            var alfabeto= this.simplificarAlfabeto(this.alfabeto1); //['a','b','c']
            var matAuto1= this.crearMatrizTransiciones(this.transicionesAutomata1); 
            var matAuto2= this.crearMatrizTransiciones(this.transicionesAutomata2); 
            var est1 = this.estadosAutomata1;
            var est2 = this.estadosAutomata2;
            var estadosaux = [] // [ inicio, aux]  [1, [[a],[1,2,3]]]
            var aux = [];       // transicion, fin  [a], [1,2,3]
            var fin= [];        //finales [ 1,2,3 ]
            var matriz = [];     //largo estados * Largo Alfabeto
            //console.log("ALFABETOOO",alfabeto);
            
            for(var i=1; i<est1.length;i++){ //inicio
                estadosaux.push(est1[i].id);
                for(var j=0; j<alfabeto.length; j++){ //alfabeto
                    aux.push(alfabeto[j]);
                    for(var k=1; k<matAuto1[0].length; k++){  //fin
                        if(matAuto1[1][k].length>1){
                            for(var m=0; m < matAuto1[1][k].length; m++){
                                if(est1[i].id == matAuto1[0][k] && matAuto1[1][k][m] == alfabeto[j]){
                                    fin.push(matAuto1[2][k]);
                                }
                            }
                        }else{
                            if(est1[i].id == matAuto1[0][k] && matAuto1[1][k] == alfabeto[j]){
                                fin.push(matAuto1[2][k]);
                            }
                        }
                    }
                    aux.push(fin);
                    fin=[];
                    estadosaux.push(aux);
                    aux=[];
                }
                matriz.push(estadosaux)
                estadosaux = []
            }
            console.log(matriz);
            this.matriz1AFND=matriz;
        },

        estadoSiguiente(matriz2, abc){
            console.log("inicio funcion estadoSiguiente");
            var inicio=[];
            var cond=false;
            var estaux=[];
            var matriz3=matriz2;
            var largo=matriz2.length;
            var arrayaux=[];
            var strAux;
            for(let j=0; j<largo; j++){
                for(let i=1; i<=abc.length; i++){
                    if(matriz2[j].length>1){
                        inicio=matriz2[j][i][1];
                        console.log("inicio adentro:", inicio);
                        for( let k=0; k<matriz2.length; k++){
                            if(this.compararArray(inicio, matriz2[k][0])){
                                cond=true;
                            }else{
                            arrayaux=arrayaux.concat(inicio);
                            }
                        }
                    }
                }
                for(var z=0; z<arrayaux.length;z++)
                {
                    if(arrayaux[z].length>1)
                    {
                        arrayaux[z]=arrayaux[z].sort();
                    }
                }

                arrayaux=this.simplificarArray(arrayaux);
                console.log("arrayaux: ", arrayaux);
                for(let y=0; y<arrayaux.length; y++){
                    if(!cond && arrayaux[y].length!=null){
                        console.log("arrayaux[y] >>>", arrayaux[y]);
                        estaux.push(arrayaux[y]);
                        console.log("estaux adentro: ",estaux)
                    }
                }
                arrayaux=[];
                console.log("estaux", estaux);
                strAux=estaux[0];
                for(let u=1; u<estaux.length; u++){
                    strAux=strAux+','+estaux[u];
                }
                inicio=[];
                inicio.push(strAux);
                let vacio=[];
                for(let p=0; p<abc.length; p++){
                    inicio.push(vacio);  
                }
                matriz3.push(inicio);
                console.log("matriz3: ",matriz3);
                estaux=[];
                inicio=[];
                cond=false;
            }
            console.log("fin estadoSiguiente");
            return matriz3;
        },

        compararArray(array1, array2){
            console.log("inicio compararArry");
            console.log("array 1", array1);
            console.log("array 2", array2);
            if(array1 == undefined || array2 == undefined){
                return true;
            }
            if(array1.includes(',')){
                array1=array1.split(',');
            }
            if(array2.includes(',')){
                array2=array2.split(',');
            }
            array1=this.simplificarArray(array1);
            array2=this.simplificarArray(array2);
            var largo1=array1.length;
            var largo2=0;
            if(array1.length>1 && array2.length>1){
                console.log("array 1", array1);
                console.log("array 2", array2);
                for(let i=0;i<array2.length;i++){
                    console.log("include >>>>>>>>", array2[i]);
                    if(array1.includes(array2[i])){
                        largo2++;
                    }
                }
            }else{
                if(array1==array2){
                    largo2=largo1;
                }
            }

            if(largo2==largo1 && array1.length>0 && array2.length>0){
                console.log("comparar Array: IGUALES", array1, array2);
                return true;
            }else{
                console.log("comparar Array: DIFERENTES", array1, array2);
                return false;
            }
        },
 
        buscarTransicion(matriz1, matriz2, abc){
            console.log("inicio buscarTransicion");
            var transicion=[];
            var transicionxABC=[];
            for(let i=0; i<matriz2.length; i++){
                console.log("matriz2[i].length == 1", matriz2[i][1].length);
                if(matriz2[i][1].length==0){   //aca revisar
                    console.log("entro if 1");
                    if(matriz2[i][0].length>1){  // o aca jej 
                        console.log("entro if 2 >>", matriz2[i][0]);
                        for(let q=1; q<=abc.length;q++)
                        {
                            matriz2[i].splice(1,1);
                        }
                        for(let k=1; k<=abc.length; k++){
                            for(let j=0; j<matriz2[i][0].length; j++){
                                for(let h=0; h<matriz1.length; h++){
                                    if(matriz2[i][0][j]==matriz1[h][0]){  
                                        console.log("matriz1[h][k][1]: ", matriz1[h][k][1]);
                                        transicion=transicion.concat(matriz1[h][k][1]);
                                    }
                                }
                            }
                            transicion=this.simplificarArray(transicion);
                            transicionxABC.push(abc[k-1]);
                            transicionxABC.push(transicion);
                            console.log("XABC", transicionxABC);
                            matriz2[i].push(transicionxABC);
                            transicion=[];
                            transicionxABC=[];
                        }
                    }else{
                        for(let q=1; q<=abc.length;q++)
                        {
                            matriz2[i].splice(1,1);
                        }
                        for(let k=1; k<=abc.length; k++){
                            for(let h=0; h<matriz1.length; h++){
                                if(matriz2[i][0]==matriz1[h][0]){  
                                    console.log("matriz1[h][k][1] v2: ", matriz1[h][k][1]);
                                    transicion=transicion.concat(matriz1[h][k][1]);
                                }
                            }
                            transicion=this.simplificarArray(transicion);
                            transicionxABC.push(abc[k-1]);
                            transicionxABC.push(transicion);
                            console.log("XABC v2", transicionxABC);
                            matriz2[i].push(transicionxABC);
                            transicion=[];
                            transicionxABC=[];
                        }
                        
                    }
                }
            }
            console.log("matriz2 final", matriz2);
            console.log("fin funcion buscarTRansicion");
            return matriz2;
        },

        simplificarArray(array){
            console.log("inicio simplificarArray");
            console.log(array);
            if(array.length>1)
            {

                var newArray=[]; 
                if(array.includes(',')){
                    array=array.split(',');
                }
                array=array.sort();
                
                for(var j=0; j<array.length-1; j++)
                {
                    if(array[j] != array[j+1])
                    {
                        newArray.push(array[j]);
                    }
                }
                newArray.push(array[array.length-1]);
                
                

                console.log("fin simplificarArray", newArray);
                return newArray;
            }
            else{
                console.log("fin simplificarArray");
                return array;
            }
            
        },

        copiarAutomata(estadosIn,transicionesIn, estadosOut,transicionesOut)
        {
            var estado={id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'};
            var transiciones={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            for(var i=0; i<estadosIn.length;i++)
            {
                estado.id= estadosIn[i].id;
                estado.label= estadosIn[i].label;
                estado.color= estadosIn[i].color;
                estado.final= estadosIn[i].final;
                estado.shape= estadosIn[i].shape;
                estadosOut.push(estado);
                estado={id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'};
            }
            for(var j=0; j<transicionesIn.length; j++)
            {
                transiciones.from= transicionesIn[j].from;
                transiciones.label= transicionesIn[j].label;
                transiciones.to= transicionesIn[j].to;
                transicionesOut.push(transiciones);
                transiciones={from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            }
        },
  
        complemento(){
            this.automataComplemento=[];
            this.transicionesComplemento=[];
            if(this.selectAuto===1)
            {
                this.copiarAutomata(this.estadosAutomata1,this.transicionesAutomata1,this.automataComplemento,this.transicionesComplemento);
                for(var i=1; i<this.automataComplemento.length; i++)
                {
                    if(this.automataComplemento[i].final===true)
                    {
                        this.automataComplemento[i].final=false;
                        this.automataComplemento[i].shape='ellipse';
                        this.automataComplemento[i].color='#C52C0B';
                    }
                    else{
                        this.automataComplemento[i].final=true;
                        this.automataComplemento[i].shape='diamond';
                        this.automataComplemento[i].color='#5cb85c';
                    }
                }
            }
            else{
                this.copiarAutomata(this.estadosAutomata2,this.transicionesAutomata2,this.automataComplemento,this.transicionesComplemento);
                for(var i=1; i<this.automataComplemento.length; i++)
                {
                    if(this.automataComplemento[i].final===true)
                    {
                        this.automataComplemento[i].final=false;
                        this.automataComplemento[i].shape='ellipse';
                        this.automataComplemento[i].color='#C52C0B';
                    }
                    else{
                        this.automataComplemento[i].final=true;
                        this.automataComplemento[i].shape='diamond';
                        this.automataComplemento[i].color='#5cb85c';
                    }
                }             
            }
        },

        

        concatenacion(){
            this.automataConcatenacion=[];
            this.transicionesConcatenacion=[];
            if(this.selectAuto==1){
                var automataAux=[];
                var transicionesAux=[];
                this.automataConcatenacion=[];
                this.transicionesConcatenacion=[];
                var trans= {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                this.copiarAutomata(this.estadosAutomata1,this.transicionesAutomata1,this.automataConcatenacion,this.transicionesConcatenacion);
                this.copiarAutomata(this.estadosAutomata2,this.transicionesAutomata2,automataAux,transicionesAux);
                automataAux.splice(0,1); 
                transicionesAux.splice(0,1);
                for(var i=0; i<this.automataConcatenacion.length;i++)
                {
                    if(this.automataConcatenacion[i].final===true)
                    {
                        this.automataConcatenacion[i].final=false;
                        this.automataConcatenacion[i].shape='ellipse';
                        this.automataConcatenacion[i].color='#C52C0B';
                        trans.from=this.automataConcatenacion[i].id;
                        trans.label='ε';
                        trans.color={color:'rgb(197,44,11)'};
                        trans.to= automataAux[0].id;
                        this.transicionesConcatenacion.push(trans);
                        trans= {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                    }
                }
                
                this.automataConcatenacion=this.automataConcatenacion.concat(automataAux);
                this.transicionesConcatenacion=this.transicionesConcatenacion.concat(transicionesAux);    
            }
            else{ 
                var automataAux=[];
                var transicionesAux=[];
                var trans= {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                this.copiarAutomata(this.estadosAutomata2,this.transicionesAutomata2,this.automataConcatenacion,this.transicionesConcatenacion);
                this.copiarAutomata(this.estadosAutomata1,this.transicionesAutomata1,automataAux,transicionesAux);
                automataAux.splice(0,1); 
                transicionesAux.splice(0,1);
                for(var i=0; i<this.automataConcatenacion.length;i++)
                {
                    if(this.automataConcatenacion[i].final===true)
                    {
                        this.automataConcatenacion[i].final=false;
                        this.automataConcatenacion[i].shape='ellipse';
                        this.automataConcatenacion[i].color='#C52C0B';
                        trans.from=this.automataConcatenacion[i].id;
                        trans.label='ε';
                        trans.color={color:'rgb(197,44,11)'};
                        trans.to= automataAux[0].id;
                        this.transicionesConcatenacion.push(trans);
                        trans= {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                    }
                }
                
                this.automataConcatenacion=this.automataConcatenacion.concat(automataAux);
                this.transicionesConcatenacion=this.transicionesConcatenacion.concat(transicionesAux);    
            }
            console.log("estados concat", this.automataConcatenacion);
            console.log("transiciones concat: ", this.transicionesConcatenacion);
        },

        interseccion(){
            if(!this.compararAlfabetos())
            {
                this.$swal("Los alfabetos de ambos automatas deben ser iguales",{
                    className: "alertas",
                    button: "Aceptar",
                    title:"Error"
                });
                return;
            }
            var auto1= this.transicionesAutomata1;
            var auto2 =this.transicionesAutomata2;
            console.log("alfabeto 1", this.alfabeto1);
            console.log("alfabeto 2", this.alfabeto2);
            var autoUnion = '';
            var autoaux = {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'}
            this.alfabeto1=this.simplificarAlfabeto(this.alfabeto1);
            this.alfabeto2=this.simplificarAlfabeto(this.alfabeto2);
            if(this.compararAlfabetos()){
                var nuevoAlfabeto = this.alfabeto1;
                for(var i=1 ; i<this.estadosAutomata1.length ;i++){
                    for(var k=1; k<this.estadosAutomata2.length; k++){
                        autoaux.label=this.estadosAutomata1[i].label+this.estadosAutomata2[k].label;
                        autoaux.id=this.estadosAutomata1[i].label+','+this.estadosAutomata2[k].label;
                        if(this.estadosAutomata1[i].final==true && this.estadosAutomata2[k].final==true){
                            autoaux.final=true;
                            autoaux.shape='diamond';
                            autoaux.color='#5cb85c';
                        }
                        this.automataInterseccion.push(autoaux);
                        console.log(this.automataInterseccion);  
                        
                        autoaux = {id:'', label:'',color:'#C52C0B', final:false, shape:'ellipse'};
                    }
                }        
                this.matrizTransicionesInterseccion();
                this.transicionInterseccion.from='inicio';
                this.transicionInterseccion.label='';
                this.transicionInterseccion.to=this.automataInterseccion[1].id;
                this.transicionesInterseccion.push(this.transicionInterseccion);
                console.log(this.transicionesInterseccion);
            }
            else{
                var nuevoAlfabeto = this.alfabeto1.concat(this.alfabeto2);
                var purgado= this.simplificarAlfabeto(nuevoAlfabeto);
                nuevoAlfabeto=purgado;
                console.log("nuevo Alfabeto:",nuevoAlfabeto);
                for(var i=1; i<this.estadosAutomata1.length; i++)
                {
                    for(j=1;j<this.estadosAutomata2.length;j++)
                    {
                        
                    }
                }
            }
        },

        matrizTransicionesInterseccion()
        {
            console.log("funcion matriz transiciones intersección");
            var matAuto1= this.crearMatrizTransiciones(this.transicionesAutomata1);
            var matAuto2= this.crearMatrizTransiciones(this.transicionesAutomata2); 
            console.log("matAuto1", matAuto1);
            console.log("matAuto2", matAuto2);
            var strId;
            var matrix=[];
            var union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
            var transicion, transicion2;
            var to1, to2;
            console.log("automata intersección", this.automataInterseccion);
            for(let i=1;i<this.automataInterseccion.length; i++){
                strId = this.automataInterseccion[i].id.split(','); 
                console.log("id automata interseccion",this.automataInterseccion[i].id);
                for(var j=1; j<matAuto1[0].length;j++)
                {   
                    console.log("Compara", strId[0]," con ",matAuto1[0][j]);
                    if(strId[0]===matAuto1[0][j])
                    {
                        console.log("strId", strId[0], strId[1]);
                        transicion=matAuto1[1][j];
                        console.log("transicion",transicion);
                        to1=matAuto1[2][j];
                        if (this.encontrarComa(transicion)){ 
                            var arraytransicion= transicion.split(',');
                            for(var m=0; m<arraytransicion.length; m++){
                                transicion= arraytransicion[m];
                                for(var k=1; k<matAuto2[2].length;k++){
                                    transicion2=matAuto2[1][k];
                                    console.log("transicion",transicion2);
                                    if(this.encontrarComa(transicion2)){   //a,b - a,b
                                        var arraytransicion2= transicion2.split(',');
                                        for(var o=0; o<arraytransicion2.length; o++){
                                            transicion2= arraytransicion2[o];
                                            if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                                to2=matAuto2[2][k];
                                                union.from=strId[0]+','+strId[1];
                                                union.label=transicion;
                                                union.to=to1+','+to2;
                                                console.log("union 1 from",union.from);
                                                console.log("union 1 transicion",union.label);
                                                console.log("union 1 to",union.to);
                                                matrix.push(union);
                                                union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                            }
                                        }
                                    }else{                              //a,b - a
                                        if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                            to2=matAuto2[2][k];
                                            union.from=strId[0]+','+strId[1];
                                            union.label=transicion;
                                            union.to=to1+','+to2;
                                            console.log("union 2 from",union.from);
                                            console.log("union 2 transicion",union.label);
                                            console.log("union 2 to",union.to);
                                            matrix.push(union);
                                            union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                        }
                                    }
                                    transicion2=null;
                                }
                                transicion=null;
                            }

                        }else{ //a - a, b      y a - a
                            for(var k=1; k<matAuto2[2].length;k++){
                                transicion2=matAuto2[1][k];
                                console.log("transicion",transicion2);
                                if(this.encontrarComa(transicion2)){   //a - a,b
                                    var arraytransicion2= transicion2.split(',');
                                    for(var o=0; o<arraytransicion2.length; o++){
                                        transicion2= arraytransicion2[o];
                                        if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                            to2=matAuto2[2][k];
                                            union.from=strId[0]+','+strId[1];
                                            union.label=transicion;
                                            union.to=to1+','+to2;
                                            console.log("union 3 from",union.from);
                                            console.log("union 3 transicion",union.label);
                                            console.log("union 3 to",union.to);
                                            matrix.push(union);
                                            union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                        }
                                    }
                                }else{                              //a - a
                                    if(strId[1]===matAuto2[0][k] && transicion2==transicion){
                                        to2=matAuto2[2][k];
                                        union.from=strId[0]+','+strId[1];
                                        union.label=transicion;
                                        union.to=to1+','+to2;
                                        console.log("union 4 from",union.from);
                                        console.log("union 4 transicion",union.label);
                                        console.log("union 4 to",union.to);
                                        matrix.push(union);
                                        union = {from:'',label: '',to:'',color:{color:'rgb(0,0,0)'}};
                                    }
                                }
                                transicion2=null;
                            }
                        }
                    }
                }           
            }
            console.log("matrix", matrix);
            this.transicionesInterseccion=matrix;
        },

        simplificarAFD(estadaux,transaux){
            this.automataSimplificado=[];
            this.transicionesSimplificado=[];
            //var estados=[{id:'inicio', label:'inicio',color:'#75616b47', final:false},{id:'1', label:'1',color:'#75616b47', final:false},{id:'2', label:'2',color:'#75616b47', final:true},{id:'3', label:'3',color:'#75616b47', final:true},{id:'4', label:'4',color:'#75616b47', final:true},{id:'5', label:'5',color:'#75616b47', final:true}]
            //var transiciones = [{from:'inicio',label: '',to:'5',color:{color:'rgb(0,0,0)'}},{from:'5',label: 'a',to:'4',color:{color:'rgb(0,0,0)'}},{from:'5',label: 'b',to:'3',color:{color:'rgb(0,0,0)'}},{from:'4',label: 'a',to:'4',color:{color:'rgb(0,0,0)'}},{from:'4',label: 'b',to:'2',color:{color:'rgb(0,0,0)'}},{from:'3',label: 'a',to:'4',color:{color:'rgb(0,0,0)'}},{from:'3',label: 'b',to:'1',color:{color:'rgb(0,0,0)'}},{from:'2',label: 'a',to:'4',color:{color:'rgb(0,0,0)'}},{from:'2',label: 'b',to:'1',color:{color:'rgb(0,0,0)'}},{from:'1',label: 'a',to:'1',color:{color:'rgb(0,0,0)'}},{from:'1',label: 'b',to:'1',color:{color:'rgb(0,0,0)'}}]
            var transiciones=[];
            var estados = [];
            this.copiarAutomata(estadaux,transaux,estados,transiciones);
            var matTrans = this.crearMatrizTransiciones(transiciones)
            //nsole.log("matriz transiciones",matTrans);
            var res = [];
            // Se crea la matriz
            for(var i=0; i<estados.length;i++){
                res[i]= new Array(estados.length);
            }
            //console.log("res1",res);
            //Se marcan con x todos los estados que no pueden simplificar entre si y con 0 la diagonal
            for(var n=0; n<estados.length; n++){
                //console.log("entro1");
                for(var m=0; m<estados.length; m++){
                    //console.log("entro2");
                    if(n==m){
                        //console.log("if1");
                        console.log(res[n]);
                        res[n][m]='x';
                    }
                    else{
                        if(estados[n].final!=estados[m].final){
                            //console.log("if2");
                            res[n][m]='x';
                            res[m][n]='x';
                        }
                        if(estados[n].final==estados[m].final){
                            res[n][m]='-';
                            res[m][n]='-';
                        }
                    }
                }
            }
            //hasta acá se creó la matriz
            //console.log("res",res); //imprime matriz
            //analisis de matriz
            for(var k=0; k<estados.length; k++){
                for(var l=0; l<estados.length; l++){
                    if(res[k][l]=='-' && res[l][k]=='-'){
                        //console.log("k-l:",k,"-",l);
                        //console.log("res[k][l]",res[k][l]);
                        for (let index = 0; index < matTrans[0].length; index++) {
                            if(k==matTrans[0][index]){
                                //console.log("trans",matTrans[0][index],matTrans[1][index],matTrans[2][index]);
                                for(let jdex=0 ;jdex <matTrans[0].length;jdex++){
                                    if(l==matTrans[0][jdex]){
                                        //console.log("trans2",matTrans[0][jdex],matTrans[1][jdex],matTrans[2][jdex]);
                                        if(matTrans[2][index] != matTrans[2][jdex] && matTrans[1][index]==matTrans[1][jdex]){
                                            var aux = []
                                            aux.push(matTrans[2][index])
                                            aux.push(matTrans[2][jdex])
                                            //console.log("coordenadas matriz:",aux);  
                                            //comparacion de variables                                       
                                            if(res[aux[0]][aux[1]]=='x' && aux[0]!=aux[1]){
                                                //console.log("entra");
                                                res[k][l]='x'
                                                res[l][k]='x'
                                            }            
                                        }
                                    }
                                }                                
                            }                            
                        }                        
                    }
                }
            }
            console.log("res",res);
            this.matrizAFD=res
            this.drawMinAfd(estados,transiciones);
            
        },
        drawMinAfd(estados,transiciones){
            var matriz = this.matrizAFD;
            var inicial;
            console.log("matriz",matriz);
            console.log("trans",transiciones);
            for(let i=0; i<matriz.length;i++){
                for(let j=0; i<matriz.length;i++){
                    if(matriz[i][j]=='-'){
                        console.log("indice" ,i,j);

                        for(var m=0 ; m<estados.length;m++){
                            console.log("estados eliminados",estados[m].id);
                            if(estados[m].id=="inicio"){
                                
                                for(let h=0;h<transiciones.length;h++){
                                    if(transiciones[h].from == estados[m].id){
                                        inicial= transiciones[h].to
                                        console.log("inicial",inicial);
                                              
                                    }
                                }
                            }
                            else{
                                if(estados[m].id==j && estados[m].id!="inicio"){
                                    console.log("estado para eliminar:",estados[m].id);
                                    for(let k=1 ; k<transiciones.length; k++){
                                        
                                        if(estados[m].id==transiciones[k].from && transiciones[k].from!="inicio"){ //1 a 3 ,1 b 2 del // 4 a 1, 5 b 2 //4 a 4 5 b 5 
                                                transiciones.splice(k,1); //(?)
                                        }
                                        else{
                                            if(transiciones[k].to==estados[m].id && transiciones[k].from!="inicio"){
                                                
                                                transiciones[m].to=transiciones[k].from                                            
                                            }
                                        }
                                    }                                
                                    estados.splice(m,1);
                                }
                            }
                        } 
                    }
                }
            }
            console.log(estados);
            console.log(transiciones);
            this.copiarAutomata(estados,transiciones,this.automataSimplificado,this.transicionesSimplificado);
            var existe=false;
            /*for(var h=0; h<this.automataSimplificado.length;h++)
            {
                for(var t=0; t<this.transicionesSimplificado.length;t++)
                {
                    if(this.automataSimplificado[h].id ===this.transicionesSimplificado[t].from)
                    {
                        existe=true;
                    }
                    else{
                        if(this.automataSimplificado[h].id != this.transicionesSimplificado[t].from && existe ===true)
                        {
                            existe=true;
                        }
                        else{
                            if(this.automataSimplificado[h].id != this.transicionesSimplificado[t].from && existe ===false)
                            {
                                existe=false;
                            }
                        }
                    }
                }
                if(!existe)
                {
                    this.automataSimplificado.splice(h,1);
                }
            } */
            console.log(this.automataSimplificado);
            console.log(this.transicionesSimplificado);
            this.dibujarSimplificado=true;
            this.drawAutomata();
        },


    },

}
</script>