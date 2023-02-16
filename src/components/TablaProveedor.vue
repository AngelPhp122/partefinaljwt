<template>
<div>
  <div>
    <v-btn color="green" @click="agregarProveedor()">Agregar</v-btn><br><br>
     <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="dataProveedor"
      :search="search"
    >
    </v-data-table>
  </v-card>
  </div>
<AgregarProveedor v-if="agregar==true" v-bind:ventanaAbrir="ventana=true"/>
  
</div>
</template>

<script>
import AgregarProveedor from '../components/AgregarProveedor.vue';
import EditarProveedor from '../components/EditarProveedor.vue';
import BorrarProveedor from '../components/BorrarProveedor.vue';

export default {

components:{
    AgregarProveedor,
    EditarProveedor,
    BorrarProveedor
},

data() {
    return{
        search:'',
        headers:[
            {
                text:'Id',
                align:'start',
                filterable:false,
                value: 'id_proveedor'
            },
            {text:'Proveedor', value: 'nombre_proveedor'},
            {text:'Direccion', value:'direccion_proveedor'},
            {text:'Telefono', value:'telefonoprov'},
            {text:'Correo', value:'correoproveedor'}
        ],
        dataProveedor:[
           /* {
                id:1,
                proveedor:'Prueba',
                direccion:'Desde la republica de El Salvador en la America Central',
                telefono: '2222-2222',
                correo:'prueba@mail.com'
            }*/
        ],

        agregar: false,
        redirigir: false,
        ventana:false


    }
},

methods:{
    agregarProveedor(){
        this.agregar = true
        this.redirigir = true

        return true;
    }

},

mounted(){
        
        const token = localStorage.getItem("token");
        fetch("https://localhost:7028/api/proveedor",{
            method:'GET',
            headers:{
                'Authorization':`Bearer ${token}`
            }
        })
        .then(respuesta => respuesta.json())
        .then(response => {
            this.dataProveedor = response;
            console.log(this.dataProveedor)
        });
    
    },

    updated(){
        const token = localStorage.getItem("token");
        fetch("https://localhost:7028/api/proveedor",{
            method:'GET',
            headers:{
                'Authorization':`Bearer ${token}`
            }
        })
        .then(respuesta => respuesta.json())
        .then(response => {
            this.dataProveedor = response;
        });
    }

}



</script>

<style>

</style>