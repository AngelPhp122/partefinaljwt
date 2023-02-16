<template>
  <div>
    <h1>Login usuario</h1>
    <v-col cols="10"
           sm="5"
           md="5">
           
    <v-text-field label="Usuario" id="usuario"></v-text-field>

    </v-col>
    <v-col cols="10"
           sm="5"
           md="5">
           
           <v-text-field label="Clave" id="clave"></v-text-field>

           </v-col>   


           <v-btn @click="logueUsuario">Login</v-btn>
  </div>
</template>

<script>
export default {

    data(){
        return{
            sesion: false
        }
    },

    methods:{
        async logueUsuario(){

            let nombreUsuario = document.getElementById('usuario').value;
            let claveUsuario = document.getElementById('clave').value;

            const oUsuario = {
                username: nombreUsuario,
                password: claveUsuario
            }

            await fetch("https://localhost:7028/api/login",{
                method:'POST',
                body: JSON.stringify(oUsuario),
                headers:{
                    'Content-Type':'application/json',
                }
            })
            .then(resp=>resp.text())
            .then(respuesta=>{
                console.log(respuesta)
                if(respuesta != 'cagaste'){
                    let token = respuesta;
                    localStorage.setItem("token",token);

                    this.sesion = true;
                }else{
                    console.log(false);
                }
            });
                this.$emit('logueo', this.sesion);
        }
    }

}
</script>

<style>

</style>