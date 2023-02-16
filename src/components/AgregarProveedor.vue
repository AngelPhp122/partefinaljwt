<template>
<div>

<v-dialog
    v-model="ventanaAbrir"
    width="500">
    
<v-card>
    <v-card-text>
        <v-row>
            <v-col>
            <v-text-field id="Proveedor" label="Proveedor" v-bind:rules="[v => !!v || 'Campo obligatorio']"></v-text-field>
            </v-col>
            <v-col>
            <v-text-field id="Direccion" label="Direccion" v-bind:rules="[v => !!v || 'Campo obligatorio']"></v-text-field>
            </v-col>
            <v-col>
                <v-text-field id="Telefono" label="Telefono" v-bind:rules="[v => !!v || 'Campo obligatorio']"></v-text-field>
            </v-col>
            <v-col>
                <v-text-field id="Correo" label="Correo" v-bind:rules="[v => !!v || 'Campo obligatorio']"></v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-select
                
                id="Seleccion"
                v-bind:items="items"
                label = "Departamento"
                v-model="selectItem"
                @click="cargarDataSelect1"
                ></v-select>
            </v-col>
            <v-col>
                <v-select
                id="Seleccion2"
                v-bind:items="municipios"
                label="Municipio"
                
                ></v-select>
            </v-col>
        </v-row>
        <v-row>
            <v-col><v-btn color="green" @click="guardarProveedor">Guardar</v-btn></v-col>
            <v-col><v-btn color="red">Cancelar</v-btn></v-col>
        </v-row>
    </v-card-text>
</v-card>
    
    </v-dialog>

</div>
    
</template>

<script>
export default {

props:['ventanaAbrir'],

data(){
    return{
            items:[],
            selectItem:'',
            municipios:[],
            arragloRespaldo:[]
    }
},

methods:{

    async guardarProveedor(){

        const token = localStorage.getItem('token')
        
        let nombre_proveedor =   document.getElementById('Proveedor').value;
        let direccion_proveedor =   document.getElementById('Direccion').value;
        let telefonoprov  = document.getElementById('Telefono').value;
        let correoproveedor =    document.getElementById('Correo').value;
       //     document.getElementById('Seleccion').value,
       //     document.getElementById('Seleccion2').value

        
       let proveedor = {
            id_proveedor: 0,
            nombre_proveedor: nombre_proveedor,
            estado_proveedor: 1,
            direccion_proveedor: direccion_proveedor,
            telefonoprov: telefonoprov,
            correoproveedor: correoproveedor,
            id_departamento_prov: 1
       } 
        
       await fetch("https://localhost:7028/api/proveedor",{
            method:'POST',
            headers: {'Authorization':`Bearer ${token}`,
                      'Content-Type':'application/json'
            },
            body: JSON.stringify(proveedor)
        })            
        .then(resp => resp.json())
        .then(console.log)


    },

    async cargarDataSelect1(){

            const token = localStorage.getItem('token')
            let array = [];

            
       if(this.items.length <= 0){

              fetch("https://localhost:7028/api/Departamento",{
            method:'GET',
            headers:{
            'Authorization':`bearer ${token}`,
            'Content-Type':'application/json'}
        })
        .then(resp => resp.json())
        .then(response =>{
            array = response;
            this.arragloRespaldo = array;

                for(let i = 0; i <= array.length; i++){
                    this.items.push( array[i].nombre_departamento);
                }
        })
       }
      
    }

},

watch:{
    selectItem: function(value){
        this.municipios = [] //este se utiliza para poder resetear cuando se cambie de departamento

        let array2 = this.arragloRespaldo;

        let busqueda = 0;

        for(let i=0; i<= array2.length; i++){
            if(array2[i].nombre_departamento.includes(value)){
                busqueda = array2[i].id_departamento;

                break;
            }
        }

        const token = localStorage.getItem('token')
        let array = [];
        fetch(`https://localhost:7028/api/municipio/${busqueda}`,{
            method:'GET',
            headers:{
                'Authorization':`Bearer ${token}`,
                'Content-Type':'application/json'

            }
        })
        .then(resp => resp.json())
        .then(response =>{
            array = response;

            for(let i = 0 ; i <= array.length; i++){
                this.municipios.push(array[i].nombre_municipio);
            }
        })
    }
}

}
</script>

<style>

</style>