<template>
  <div>
    <div class="select-wrapper">
      <select v-model="opcionSeleccionada" class="select-custom">
        <option value="biseccion">Bisección</option>
        <option value="jacobi">Jacobi</option>
        <option value="seidel">Gauss Seidel</option>
        <option value="newton">Newton Raphson</option>
        <option value="punto">Punto Fijo</option>
        <option value="secante">Secante</option>
        <option value="simpson">Simpson</option>
        <option value="trapezoidal">Trapezoidal</option>
        <option value="polinomio">Polinomio de Lagrange</option>
      </select>
    </div>

    <!-- Mostrar la calculadora si la opción seleccionada es 'calculadora' -->
    <div v-if="opcionSeleccionada === 'biseccion'">
      <CalculatorComponent :typeMetod="1" titulo="Biseccion" />
    </div>
    <div v-if="opcionSeleccionada === 'newton'">
      <CalculatorComponent :typeMetod="3" titulo="Newton Raphson" />
    </div>
    <div v-if="opcionSeleccionada === 'punto'">
      <CalculatorComponent :typeMetod="4" titulo="Punto Fijo" />
    </div>
    <div v-if="opcionSeleccionada === 'secante'">
      <CalculatorComponent :typeMetod="5" titulo="Secante" />
    </div>
    <div v-if="opcionSeleccionada === 'simpson'">
      <CalculatorComponent :typeMetod="6" titulo="Simpson" />
    </div>
    <div v-if="opcionSeleccionada === 'trapezoidal'">
      <CalculatorComponent :typeMetod="7" titulo="Trapezoidal" />
    </div>

    <div v-if="opcionSeleccionada === 'jacobi'">
      <Matriz  :typeMetod="2" titulo="Jacobi"/>
    </div>
    <div v-if="opcionSeleccionada === 'seidel'">
      <Matriz  :typeMetod="8" titulo="Gauss Seidel"/>
       
    </div>
    <div v-if="opcionSeleccionada === 'polinomio'">
      <Polinomio/>

    </div>

    <div v-if="opcionSeleccionada === ''">
      <div class="main-container">
        <h1 class="page-title">Proyecto Métodos Numéricos</h1>
    
      </div>
    </div>
  </div>
</template>

<script>
import CalculatorComponent from "./CalculatorComponent.vue"; 
import Matriz from "./MatrizMetod.vue";
import Polinomio from "./PolinomioMetodo.vue";
export default {
  data() {
    return {
      opcionSeleccionada: "", 
      filas: null,
      columnas: null,
      matriz: [], 
    };
  },
  components: {
    CalculatorComponent,
    Matriz,
    Polinomio
  },
};
</script>

<style scoped>
.main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; 
  background-color: #f0f0f0; 
}

.page-title {
  font-family: 'Arial', sans-serif; 
  font-size: 36px;
  color: #333;
  text-align: center;
  text-transform: uppercase;
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2); 
  padding: 20px;
  background-color: #fff; 
  border-radius: 10px; 
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3); 
}


.select-wrapper {
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  width: 50%; 
}

.select-custom {
  font-family: "Courier New", monospace;
  font-size: 16px; 
  color: #000; 
  background-color: #fff; 
  padding: 8px 16px; 
  border: 2px solid #000; 
  border-radius: 5px; 
  cursor: pointer;
  transition: all 0.3s ease; 
}

.select-custom:hover {
  background-color: #f5f5f5; 
  border-color: #666; 
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3); 
}

.select-custom:focus {
  outline: none;
}

.select-custom::after {
  content: "\25BC"; 
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  color: #000; 
  font-size: 14px;
  pointer-events: none;
}


.select-custom option {
  font-family: "Courier New", monospace; 
  font-size: 14px; 
  background-color: #fff; 
  color: #000;
}
.calculator {
  width: 300px;
  margin: 0 auto;
}

.calculator-screen {
  background-color: #f0f0f0;
  padding: 10px;
  text-align: right;
  font-size: 24px;
}

.calculator-buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  margin-top: 10px;
}

button {
  background-color: #4caf50;
  color: #fff;
  font-size: 20px;
  padding: 10px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #45a049;
}
</style>
