<template>
  <div id="app" class="container">
    <h1>Captura de Alumnos</h1>

    <div class="card">
      <div class="card-header">
        <h3>Agrega alumno</h3>

        <div class="card-body">
          <form v-on:submit.prevent="adddata" class="form-inline">

            <div class="form-group">
              <input
              class="form-control"
              v-model="newalumno.nombre"
              placeholder="Nombre(s)"
              type="text">
            </div>

            <div class="form-group">
              <input
              class="form-control"
              v-model="newalumno.apellidoP"
              placeholder="Apellido P."
              type="text">
            </div>

            <div class="form-group">
              <input
              class="form-control"
              v-model="newalumno.apellidoM"
              placeholder="Apellido M."
              type="text">
            </div>

            <div class="form-group">
              <input
              class="form-control"
              v-model="newalumno.email"
              placeholder="Email"
              type="text">
            </div>

            <input type="submit" class="btn btn-success" value="Agrergar">

          </form>
        </div>
      </div>
    </div>

    <hr>

    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Lista de Alumnos</h3>
      </div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Nombre(s)</th>
              <th>Apellido P.</th>
              <th>Apellido M.</th>
              <th>E mail</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="alumno in alumnos">
              <td>{{ alumno.nombre }}</td>
              <td>{{ alumno.apellidoP }}</td>
              <td>{{ alumno.apellidoM }}</td>
              <td>{{ alumno.email }}</td>
              <td>
                <button
                v-on:click="deletealumno(alumno)"
                class="btn btn-danger">
                <i class="fa fa-trash-o" aria-hidden="true"></i>
                </button>
              </td>
            </tr>
          </tbody>

        </table>
      </div>
    </div>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase';
import  toastr from 'toastr';

let config = {
    apiKey: "AIzaSyBypiMn_apSlwBC8cxIYYCIjghF28BKbsQ",
    authDomain: "vuejsfire-cfbab.firebaseapp.com",
    databaseURL: "https://vuejsfire-cfbab.firebaseio.com",
    projectId: "vuejsfire-cfbab",
    storageBucket: "vuejsfire-cfbab.appspot.com",
    messagingSenderId: "925428545185"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let alumnosref = db.ref('alumnos');

export default {
  name: 'App',
  firebase: {
    alumnos: alumnosref
  },
  data(){
    return{
      newalumno: {
        nombre: '',
        apellidoP: '',
        apellidoM: '',
        email: ''
      }
    }
  },
  methods: {
    adddata: function(){
      alumnosref.push(this.newalumno);
      toastr.success('Alumno agregado');
      this.newalumno.nombre='';
      this.newalumno.apellidoP='';
      this.newalumno.apellidoM='';
      this.newalumno.email='';
    },
    deletealumno: function(alumno){
      alumnosref.child(alumno['.key']).remove();
      toastr.success('Alumno eliminado');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
