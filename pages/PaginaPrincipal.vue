<template>
<div align="center">
  <h1>Esta es la pagina principal la wea fome</h1>
  <LoginProveedor v-if="sesion == false" @logueo="sesion = $event" />
  <TablaProveedor v-else/>
</div>
</template>

<script>
import TablaProveedor from '../src/components/TablaProveedor.vue';
import LoginProveedor from '../src/components/LoginProveedor.vue';

export default {
components:{
  TablaProveedor,
  LoginProveedor
},

data() {

  return{
    sesion: false
  }
}

}
</script>

<style>

</style>