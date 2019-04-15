<template>
  <div id="app">

    <!-- Mostrará el título dictado en la data del componente. (ver más abajo) -->
    <h1>{{title}}</h1>

    <!-- Comenzará un ciclo que se repetirá cuantas veces elementos haya en el arreglo de Evaluaciones. (data del componente) -->
    <div v-for="(evaluacion, index) in Evaluaciones" :key="index">
      <v-form>
        <v-container>
          <v-layout row wrap align-center justify-center>

            <!-- Se realiza una directiva v-if: Si el valor en el campo de porcentaje es > 0,
            entonces Muestra el nombre de la evaluación y su porcentaje arriba de los campos. -->
            <h2 v-if="evaluacion.porcentaje > 0">{{evaluacion.nombre}} ({{evaluacion.porcentaje}}%)</h2>

            <v-flex xs12 sm6 md3>
              <v-text-field type="text" v-model="evaluacion.nombre" label="Tipo de Evaluación"></v-text-field>
            </v-flex>

            <v-flex xs12 sm6 md3>
              <v-text-field
                type="number"
                v-model="evaluacion.promedioNotas"
                label="Promedio de Notas"
                hint="Ingrese un valor entre 1-100"
              ></v-text-field>
            </v-flex>

            <v-flex xs12 sm6 md3>
              <v-text-field
                type="number"
                v-model="evaluacion.porcentaje"
                label="Porcentaje total de Notas (%)"
                :hint="[[porcentajeActual]] + '/100%'"
              ></v-text-field>
            </v-flex>
          </v-layout>
        </v-container>
      </v-form>
    </div>

    <!-- Botón que calcula el Promedio Final. Aparece cuando el porcentaje total de notas equivale a 100. -->
    <v-layout column wrap align-center>
      <v-btn
        round
        color="blue"
        dark
        v-if="porcentajeActual == 100"
        @click="calcularPromedio"
      >Calcular Mi Promedio Final</v-btn>

    <!-- Botón que remueve una evaluación. Aparece cuando hay evaluaciones. -->
      <v-btn
        v-if="Evaluaciones.length > 0"
        round
        outline
        color="red"
        @click="removerEvaluacion"
      >Remover Evaluación</v-btn>

    <!-- Botón que agrega una evaluación -->
      <v-btn round outline color="indigo" @click="agregarEvaluacion">Agregar Evaluación</v-btn>

    <!-- Botón que resetea el programa. Ap -->
      <v-btn
        v-if="seCalculoUnPromedio"
        round
        outline
        color="green"
        @click="calcularNextMateria"
      >Calcular otra Materia</v-btn>
    
    <!-- Datos de salida que se muestran dependiendo del valor del porcentajeTotal calculado. -->
      <h1 v-if="porcentajeTotal == 100">Promedio Final: {{promedioFinal}}/100</h1>
      <h3 v-else>Es requerido que el porcentaje total sea 100%</h3>

      <h1>{{msg}}</h1>
    </v-layout>
  </div>
</template>

<script>
export default {
  name: "app",

  data: () => ({
    title: "Programación 2: Proyecto Final",
    promedioFinal: 0,
    promedioPonderado: 0,
    porcentajeTotal: 0,
    porcentajeAcumulado: 0,
    msg: "",
    Evaluaciones: [],
    seCalculoUnPromedio: false
  }),

  computed: {

    // Propiedad computada que calcula el total de porcentaje de notas en tiempo real
    porcentajeActual() {
      var sumPorcentajesActual = 0;
      for (var a = 0; a < this.Evaluaciones.length; a++) {
        sumPorcentajesActual += parseInt(this.Evaluaciones[a].porcentaje);
      }
      if (isNaN(sumPorcentajesActual)){
        return 0;
      }
      else{
        return sumPorcentajesActual;
      }
      
    }
  },

  methods: {

    // Método de agregar una nueva evalución
    agregarEvaluacion() {
      this.Evaluaciones.push({
        nombre: "",
        promedioNotas: 0,
        porcentaje: 0
      });
    },

    removerEvaluacion() {
      this.Evaluaciones.pop();
    },

    // Método de Calcular un nuevo promedio Final. Resetea el arreglo de evaluaciones
    // y agrega una nueva evaluación
    calcularNextMateria() {
      this.seCalculoUnPromedio = false;
      this.porcentajeTotal = 0;
      this.Evaluaciones = [];
      this.Evaluaciones.push({
        nombre: "",
        promedioNotas: 0,
        porcentaje: 0
      });
    },

    // En cada Evaluación, multiplica cada promedio de notas con su porcentaje
    // y suma los porcentajes.
    calcularPromedio() {
      this.seCalculoUnPromedio = true;

      var sumTotal = 0;
      var sumPorcentajes = 0;

      for (var i = 0; i < this.Evaluaciones.length; i++) {
        sumTotal += (Number(this.Evaluaciones[i].promedioNotas) * Number(this.Evaluaciones[i].porcentaje)) /100;
        sumPorcentajes += Number(this.Evaluaciones[i].porcentaje);
      }
      this.promedioFinal = Math.round(sumTotal*100)/100;
      this.porcentajeTotal = sumPorcentajes;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 25px;
}
ul {
  position: fixed;
  top: 0;
  list-style-type: none;
  margin: 0;
  padding: 0;
  width: 100%;
  background-color: #dddddd;
  overflow: hidden;
  text-align: center;
}
li {
  display: inline;
}
li a {
  display: inline-block;
  color: white;
  padding: 14px 16px;
  text-decoration: none;
}
li:last-child {
  border-right: none;
}
li a:hover {
  background-color: #555;
  color: white;
}
.nav-bar {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 25px;
}
</style>
