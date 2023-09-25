<template>
  <div>
    <div class="select-wrapper">
      <select v-model="opcionSeleccionada" class="select-custom">
        <option value="biseccion">Bisección</option>
        <option value="jacobi">Jacobi</option>
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

    <div v-if="opcionSeleccionada === 'jacobi'">
      <CalculatorComponent :typeMetod="2" titulo="Jacobi" />
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
    <div v-if="opcionSeleccionada === 'polinomio'">
      <CalculatorComponent :typeMetod="8" titulo="Polinomio de lagrange" />
    </div>

    <div v-if="opcionSeleccionada === ''">
      <div class="main-container">
        <h1 class="page-title">Proyecto Métodos Numéricos</h1>
    
      </div>
    </div>
  </div>
</template>

<script>
import CalculatorComponent from "./CalculatorComponent.vue"; // Asegúrate de importar el componente de la calculadora aquí

export default {
  data() {
    return {
      opcionSeleccionada: "", // Opción predeterminada
      filas: null, // Variable para almacenar el número de filas de la matriz
      columnas: null, // Variable para almacenar el número de columnas de la matriz
      matriz: [], // Matriz para almacenar los valores ingresados por el usuario
    };
  },
  components: {
    CalculatorComponent,
  },
};
</script>

<style scoped>
.main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Establece la altura para ocupar toda la altura de la ventana */
  background-color: #f0f0f0; /* Fondo de la página */
}

.page-title {
  font-family: 'Arial', sans-serif; /* Cambia la fuente a Arial o la que prefieras */
  font-size: 36px; /* Tamaño de fuente grande */
  color: #333; /* Color del texto */
  text-align: center; /* Centra el texto horizontalmente */
  text-transform: uppercase; /* Convierte el texto en mayúsculas */
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2); /* Agrega sombra al texto */
  padding: 20px; /* Espaciado interno */
  background-color: #fff; /* Fondo del título */
  border-radius: 10px; /* Bordes redondeados */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3); /* Agrega sombra al título */
}


.select-wrapper {
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  width: 50%; /* O ajusta el ancho que desees */
}

.select-custom {
  font-family: "Courier New", monospace; /* Cambia la fuente a Courier New o la que prefieras */
  font-size: 16px; /* Tamaño de fuente */
  color: #000; /* Texto en negro */
  background-color: #fff; /* Fondo blanco */
  padding: 8px 16px; /* Espaciado interno */
  border: 2px solid #000; /* Borde con color negro */
  border-radius: 5px; /* Bordes redondeados */
  cursor: pointer;
  transition: all 0.3s ease; /* Transición suave */
}

.select-custom:hover {
  background-color: #f5f5f5; /* Cambia el color de fondo al pasar el mouse */
  border-color: #666; /* Cambia el color del borde al pasar el mouse */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3); /* Agrega sombra al pasar el mouse */
}

.select-custom:focus {
  outline: none; /* Elimina el contorno predeterminado al enfocar */
}

/* Estilo de flecha desplegable */
.select-custom::after {
  content: "\25BC"; /* Código de flecha hacia abajo */
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  color: #000; /* Color de la flecha (negro) */
  font-size: 14px;
  pointer-events: none;
}

/* Estilo de opciones desplegables */
.select-custom option {
  font-family: "Courier New", monospace; /* Cambia la fuente de las opciones */
  font-size: 14px; /* Tamaño de fuente de las opciones */
  background-color: #fff; /* Color de fondo de las opciones (blanco) */
  color: #000; /* Color del texto de las opciones (negro) */
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
