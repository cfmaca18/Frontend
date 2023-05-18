<template>
  <div>
    <h2 class="p-3 text-center">Agregar miembro</h2>
    <div class="container">
      <form class="form-inline" @submit.prevent="agregarMiembro">
        <div class="row">
          <div class="col-md-4 mb-3">
            <label for="estado-select">Estado:</label>
            <select class="form-control" id="estado-select" v-model="inscrito.estado" >
              <option value="activo">Activo</option>
              <option value="inactivo">Inactivo</option>
            </select>
          </div>
          <div class="col-md-4 mb-3">
            <label for="grupo-select">Grupo:</label>
            <select class="form-control" id="grupo-select" v-model="inscrito.nombre_grupo" >
              <option v-for="nombre_grupo in grupos" :key="nombre_grupo.id" :value="nombre_grupo.url">{{ nombre_grupo.nombre_grupo }}</option>
            </select>
          </div>
        </div>
        <div class="row">
          <div class="col-md-4 mb-3">
            <label for="perfil-select">Perfil:</label>
            <select class="form-control" id="perfil-select" v-model="inscrito.perfil" >
              <option v-for="perfil in perfiles" :key="perfil.id" :value="perfil.url">{{ perfil.rol }} </option>
            </select>
          </div>
          <div class="col-md-4 mb-3">
            <label for="ficha-select">Ficha:</label>
            <select class="form-control" id="ficha-select" v-model="inscrito.ficha" >
              <option v-for="ficha in fichas" :key="ficha.id" :value="ficha.url">{{ ficha.programa }}</option>
            </select>
          </div>
        </div>
        <form class="form-inline">
          <div class="col-md-4 mb-3">
            <label for="proyecto">Proyecto</label>
            <input class="form-control mx-sm-3" type="number" id="proyecto" v-model="inscrito.proyecto">
          </div>
        </form> 
        <button type="submit" class="btn btn-outline-success">Agregar miembro</button>
        <button class="btn btn-outline-danger" @click="cancelar">Cancelar</button>
      </form>
    </div>
  </div>
</template>
  
<script>
import axios from 'axios';
  
  export default{
    name: 'crear',
    data(){
      return{
        errors:[],
        inscrito:{
          estado:null,
          nombre_grupo: null,
          perfil:null,
          ficha:null,
          proyecto: null
        },
        grupos:[],
        perfiles:[],
        fichas:[]
      }
    },
    created(){
      this.obtenerGrupos();
      this.obtenerPerfiles();
      this.obtenerFichas();
    },
    
    methods:{
      async obtenerGrupos(){
        const respuesta = await axios.get('http://127.0.0.1:8000/api/grupo/');
        this.grupos = respuesta.data;
        
      },
      async obtenerPerfiles(){
        const respuesta = await axios.get('http://127.0.0.1:8000/api/perfil/');
        this.perfiles = respuesta.data;
        
      },
      async obtenerFichas(){
        const respuesta = await axios.get('http://127.0.0.1:8000/api/ficha/');
        this.fichas = respuesta.data;
        
      },
      async agregarMiembro(id){
        await axios.post('http://127.0.0.1:8000/api/inscrito/', this.inscrito);
        this.$router.push('/grupo/'+id)
      },
      cancelar() {
        this.$router.push('/CrearGrupo');
      }
      
    }

  } 
</script>


  
  