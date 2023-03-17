

<!-- tabla  -->
  <template>
      <div>
        <h1 style="color: black;">PRODUCTOS S.A.S</h1>
  
        <table class="table table-dark table-striped">
          <thead>
            <tr>
              <td>Nombre</td>
              <td>Costo</td>
              <td>Precio</td>
              <td>Cantidad</td>
              <td>Proveedor</td>
              <td>Estado</td>
              <td>Opciones</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(p, i) in productos" :key="i">
              <td>{{ p.nombre }}</td>
              <td>{{ p.costo }}</td>
  
              <td>{{ p.precio }}</td>
              <td v-bind:style="p.cantidad>50?'color:blue':p.cantidad<10?'color:orange':''">{{ p.cantidad }}</td>
              <td>{{ p.proveedor }}</td>
              <td>
                <span style="color:green" v-if="p.estado===1">Activo</span>
                <span style="color:red" v-else>Inactivo</span>
              </td>
              <td id="td">
            <button  modal=true @click="editar(p,i)" id="lapiz" type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">✏</button>
                <button id="x" type="button"  class="btn btn-Danger" @click="eliminar(i)">❌</button>
                <button  id="act" type="button"  class="btn btn-Danger" @click="habilitar(i,p)" v-if="p.estado===1">🚫</button>
                <button id="act" type="button"  class="btn btn-Danger" @click="habilitar(i,p)" v-else>✔</button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr></tr>
          </tfoot>
        </table>
  <!-- Button trigger modal  -->
  
  
  <!-- Modal  y boton nuevo registro--> 
  <button type="button" @click="limpiar() ;agregar()" class="btn btn-success" id="registro"  data-bs-toggle="modal" data-bs-target="#exampleModal " >Nuevo registro</button>
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 v-if="modal==true" class="modal-title fs-5" id="exampleModalLabel">Editar</h1>
          <h1 v-else class="modal-title fs-5" id="exampleModalLabel">Crear </h1>
          <button @click="cerrar()" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="my-3">
              <label for="">Nombre</label>
              <input v-model="nombre" class="form-control" type="text" placeholder="...">
          </div>
          <div class="my-3">
              <label for="">Costo</label>
              <input v-model="costo" class="form-control" type="number" placeholder="...">
          </div>
          <div class="my-3">
              <label for="">Precio</label>
              <input v-model="precio" class="form-control" type="number" placeholder="...">
          </div>
          
          <div class="my-3">
              <label for="">Cantidad</label>
              <input v-model="cantidad" class="form-control" type="number" placeholder="...">
          </div>
          <div class="my-3">
              <label for="">Proveedor</label>
              <input v-model="proveedor" class="form-control" type="text" placeholder="...">
          </div>
          <!-- <div v-if="valida !='' " style="width: 100%; background-color: darkred; margin-right: 20px;" class="text-center">
            <span style="color:white" >{{ valida }}</span>
          </div> -->
            <!-- <div v-show="valid==true" style="width: 100%; background-color: darkred; margin-right: 20px;" class="text-center"> -->
            <span v-if="valid===true" style="width: 100%; background-color: darkred;color:white">{{ valida }}</span>
          <!-- </div>  -->
          <div  v-show="esfumar===true"  style="width: 100%; background-color: green; margin-right: 20px;" class="text-center">
            <span  style="color:black">{{ vero }}</span>
          </div>
  
        </div>
        <div class="modal-footer">
          <input v-if="bd==true"  type="button" value="guardar" class="btn btn-success"  @click="guardar(p,i)">
          <button v-else @click="agregarValidar()" type="button" class="btn btn-success"    >Añadir</button> 
          <!-- <button v-else @click="agregarValidar()" type="button" class="btn btn-success" data-bs-dismiss="modal"  >Añadir</button>  -->
        </div>
      </div>
    </div>
  </div>
      </div>
    </template>
        
    <script>
    import {ref} from "vue"
    export default {
      setup(){
        let guard= ref(guardar.value="display: none")
        let bd= ref()
        let modal =ref()
        let valida=ref("")
        let valid=ref()
        let nombre =ref("")
        let precio=ref(null)
        let costo =ref(null)
        let cantidad=ref(null)
        let proveedor=ref("")
        let estado =ref("")
        let id=ref("")
        let esfumar = ref()
        let probando=ref("")
        let vero=ref("")
  
        let productos = ref([
        {
          
          nombre: "Bombillo",
          precio: 2000,
          costo: 1800,
          cantidad: 6,
          proveedor: "Light SA",
          estado: 0,
        },
        {
          nombre: "Shampu",
          precio: 2000,
          costo: 1800,
          cantidad: 6,
          proveedor: "Champu SA",
          estado: 1,
        },
        {
          nombre: "ramera",
          precio: 3000,
          costo: 1600,
          cantidad: 16,
          proveedor: "fabila SA",
          estado: 2,
        },
        {
          nombre: "lima",
          precio: 200,
          costo: 1800,
          cantidad: 1,
          proveedor: "limas SA",
          estado: 1,
        },
      ])
  
  
      function cerrar ()
      {console.log("modal falso"); modal.value=false} 
  
      function habilitar(i,p){ //ok
        console.log(i+ "i"); console.log(p+"p");
        if (p.estado===1){productos.value[i].estado=0}
        else{ productos.value[i].estado=1}
      }
  
      function eliminar (i){productos.value.splice(i,1) }
  
      function editar (p,i){ //ok
        modal.value=true
        bd.value = true
        console.log("entro a editar");
        vero.value=i
        nombre.value=p.nombre
        precio.value=p.precio
        costo.value=p.costo
        cantidad.value=p.cantidad
        proveedor.value=p.proveedor
        console.log(vero.value);
      }
  
      function guardar (){ //ok
        if(agregar()===true){
          console.log("entro a guardar");
        productos.value[vero.value].nombre=nombre.value
        productos.value[vero.value].precio=precio.value
        productos.value[vero.value].costo=costo.value 
        productos.value[vero.value].cantidad=cantidad.value
        productos.value[vero.value].proveedor=proveedor.value
        limpiar()
        }else{
          console.log("nq");
        }
        
      }
  
      function limpiar(){
        bd.value=false
        id.value=vero.value
        nombre.value=""
        precio.value=""
        costo.value=""
        cantidad.value=""
        proveedor.value=""
      }
  
      function agregar() { 
        valida.value=false
        console.log("entro a agregar");
      if (nombre.value.trim()===""){
        valida.value="falta nombre";
        settime()
        // valid.value=true
        // setTimeout(function(){
        //   valid.value=false
        // },3000)
      }else if(costo.value===""  && costo.value!= Number){
        valida.value="Complete el campo costo";settime()
      }else if(precio.value==="" && precio.value!= Number){
        valida.value="Complete el campo precio";settime()
      }else if (precio.value<=0){
        valida.value="Dijite un valor valido";settime()
      }else if (precio.value <=costo.value){
        valida.value="el precio debe ser mayor al costo";settime()
      }else if(cantidad.value===""&& cantidad.value!= Number){
        valida.value="Complete el campo con numeros dos";settime()
      }else if (proveedor.value.trim()===""){
        valida.value="falta Proveedor"; settime()
     }else
      return true 
    }

    function settime(){
      console.log("entro a set time");
      valid.value=true
        setTimeout(function(){
          valid.value=false
        },3000)
    }
  
      function agregarValidar (){
        agregar()
        console.log("entro a agregarValidar");
        if (agregar()=== true) {
                  console.log("entro a agregarValidar");

        productos.value.push(
        {
          nombre: nombre.value,
          precio: precio.value,
          costo: costo.value,
          cantidad: cantidad.value,
          proveedor: proveedor.value
        }
      )
      console.log(productos.value+ "agrego");
      valida.value="Registro agregado"
      esfumar.value=true
      setTimeout(function(){
        esfumar.value=false
          }, 7000);
  
          limpiar()
        }

        if(agregar()=== false){
        console.log("nada");
      }
    
    }return{
        productos,valid,cerrar,settime, habilitar,eliminar,editar,nombre,id,guardar,vero,probando,limpiar,agregar,guard,agregarValidar,
        precio,costo,cantidad,proveedor,estado,bd,valida,modal,esfumar
      }
    }
  
      }
  
    
    </script>
    <style>
    h1{
      text-align: center;
      padding: 3%;
    }
    body{
    font-family: 'Comfortaa', cursive;
    font-family: 'PT Sans Narrow', sans-serif;
    font-family: 'Rubik', sans-serif;
    }
    table{
      margin: auto;
      width: 80% !important;    
  
    }
    #registro{
      background-color: rgb(56, 66, 179);
      width: 100px;
      height: 50px;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0 auto;
      
    }
    #lapiz{
    background-color: black;
    color: #fff;
    border-color: rgba(54, 163, 52, 0.692); 
  
    }
  
  </style> 