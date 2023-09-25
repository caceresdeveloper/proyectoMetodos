<template>
  <div class="h-screen calculator-container flex justify-center py-16">
    <div class="centered-content">
      <div
        class="p-4 text-right shadow-2xl rounded-t-lg bg-white overflow-hidden max-h-96"
      >
        <p class="text-right text-gray-600 font-semibold">Resultado</p>
        <textarea
          readonly
          class="w-full h-full p-40 resize-none border border-gray-300 rounded"
          v-model="resultado"
        ></textarea>
      </div>

      <div class="grid grid-cols-12 gap-2 mt-2"></div>

      <div
        class="p-4 text-right shadow-2xl rounded-t-lg bg-white overflow-hidden max-h-96"
      >
        <p class="text-right text-gray-600 font-semibold">
          Seleccione un método
        </p>
        <select v-model="metodo" @change="enviar">
          <option value="biseccion">Bisección</option>
          <option value="jacobi">jacobi</option>
          <option value="newton">newton</option>
          <option value="polinomio">polinomio</option>
          <option value="puntofijo">punto_fijo</option>
          <option value="secante">secante</option>
          <option value="simpson">simpson</option>
          <option value="trapezoidal">trapezoidal</option>
        </select>
        <p class="font-semibold text-gray-700 h-6">{{ ecuacion }}</p>
      </div>

      <div class="centered-content">
        <button @click="agregar('x')" class="calculator-button rounded">
          x
        </button>
        <button @click="agregar('y')" class="calculator-button rounded">
          y
        </button>
        <button @click="agregar('π')" class="calculator-button rounded">
          π
        </button>
        <button @click="agregar('e')" class="calculator-button rounded">
          e
        </button>
        <button @click="agregar('7')" class="calculator-button rounded">
          7
        </button>
        <button @click="agregar('8')" class="calculator-button rounded">
          8
        </button>
        <button @click="agregar('9')" class="calculator-button rounded">
          9
        </button>
        <button @click="agregar('*')" class="calculator-button rounded">
          ×
        </button>
        <button @click="agregar('/')" class="calculator-button rounded">
          ÷
        </button>
        <button @click="agregar('sen(')" class="calculator-button rounded">
          sen
        </button>
        <button @click="agregar('cos(')" class="calculator-button rounded">
          cos
        </button>
        <button @click="agregar('tg(')" class="calculator-button rounded">
          tg
        </button>
      </div>

      <div class="centered-content">
        <button @click="agregar('**')" class="calculator-button rounded">
          **
        </button>
        <button @click="agregar('x**2')" class="calculator-button rounded">
          x<sup>2</sup>
        </button>
        <button @click="agregar('√')" class="calculator-button rounded">
          √
        </button>
        <button @click="agregar('| |')" class="calculator-button rounded">
          | |
        </button>
        <button @click="agregar('4')" class="calculator-button rounded">
          4
        </button>
        <button @click="agregar('5')" class="calculator-button rounded">
          5
        </button>
        <button @click="agregar('6')" class="calculator-button rounded">
          6
        </button>
        <button @click="agregar('+')" class="calculator-button rounded">
          +
        </button>
        <button @click="agregar('-')" class="calculator-button rounded">
          -
        </button>
        <button @click="agregar('(')" class="calculator-button rounded">
          (
        </button>
        <button @click="agregar(')')" class="calculator-button rounded">
          )
        </button>
        <button @click="agregar(';')" class="calculator-button rounded">
          ;
        </button>
      </div>

      <div class="centered-content">
        <button class="calculator-button rounded">&gt;</button>
        <button class="calculator-button rounded">&lt;</button>
        <button class="calculator-button rounded">,</button>
        <button class="calculator-button rounded">e^x</button>
        <button @click="agregar('1')" class="calculator-button rounded">
          1
        </button>
        <button @click="agregar('2')" class="calculator-button rounded">
          2
        </button>
        <button @click="agregar('3')" class="calculator-button rounded">
          3
        </button>
        <button @click="agregar('=')" class="calculator-button rounded">
          =
        </button>
        <button @click="borrarUltimoCaracter" class="calculator-button rounded">
          ⌫
        </button>
        <button class="calculator-button rounded">d/dx</button>
        <button class="calculator-button rounded">∫</button>
        <button class="calculator-button rounded">∫</button>
      </div>

      <div class="centered-content">
        <button class="calculator-button rounded">z</button>
        <button class="calculator-button rounded">x</button>
        <button class="calculator-button rounded">w</button>
        <button class="calculator-button rounded">log</button>
        <button class="calculator-button rounded">ln</button>
        <button class="calculator-button rounded">%</button>
        <button class="calculator-button rounded">AC</button>
        <button class="calculator-button rounded">.</button>
        <button class="calculator-button rounded">dx</button>
        <button class="calculator-button rounded">x/x</button>
        <button class="calculator-button rounded">º</button>
        <button @click="enviar()" class="calculator-button rounded">
          enviar
        </button>
      </div>
      <div v-if="metodo === 'biseccion'">
        <label for="inferior">a</label>
        <input v-model="a" type="number" />
        <label for="inferior">b</label>
        <input v-model="b" type="number" />
      </div>
      <div v-if="metodo === 'newton'">
        <label for="inferior">xi</label>
        <input v-model="xi" type="number" />
      </div>
      <div v-if="metodo === 'secante'">
        <label for="inferior">x0</label>
        <input v-model="x0" type="number" />
        <label for="inferior">x1</label>
        <input v-model="x1" type="number" />
      </div>
      <div v-if="metodo === 'simpson'">
        <label for="inferior">inf</label>
        <input v-model="inferior" type="number" />
        <label for="inferior">sup</label>
        <input v-model="superior" type="number" />
        <label for="intervalos">intervalos</label>
        <input v-model="intervalos" type="number" />
      </div>
      <div v-if="metodo === 'trapezoidal'">
        <label for="inferior">inf</label>
        <input v-model="inferior" type="number" />
        <label for="inferior">sup</label>
        <input v-model="superior" type="number" />
        <label for="intervalos">intervalos</label>
        <input v-model="intervalos" type="number" />
      </div>
    </div>
    <div class="w-1/2">
      <div
        class="p-4 text-right shadow-2xl rounded-t-lg bg-white overflow-hidden"
      >
        <p class="text-right text-gray-600 font-semibold">Gráfico</p>
        <canvas
          id="myChart"
          class="w-full h-full border border-gray-300 rounded"
        ></canvas>
      </div>
      <div class="grid grid-cols-12 gap-2 mt-2"></div>
    </div>
  </div>
</template>


<script >
import axios from "axios";
//import Chart from "chart.js";

export default {
  data() {
    return {
      ecuacion: "",
      a: 0,
      b: 0,
      xi: 0,
      x0: 0,
      x1: 0,
      inferior: 0,
      superior: 0,
      intervalos: 0,
      resultado: 0,
      metodo: "",
    };
  },
  methods: {
    agregar(caracter) {
      this.ecuacion = this.ecuacion + caracter;
    },
    enviar() {
      // Realizar la solicitud POST a la API utilizando Axios
      let requerimiento = { ecuacion: this.ecuacion, a: this.a, b: this.b };
      let requerimiento2 = { ecuacion: this.ecuacion, xi: this.xi };
      let requerimiento3 = { ecuacion: this.ecuacion };
      let requerimiento4 = {
        ecuacion: this.ecuacion,
        x0: this.x0,
        x1: this.x1,
      };
      let requerimiento5 = {
        ecuacion: this.ecuacion,
        inferior: this.inferior,
        superior: this.superior,
        intervalos: this.intervalos,
      };

      if (this.metodo === "biseccion") {
        axios
          .post("http://127.0.0.1:5000/api/biseccion", requerimiento, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta

          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else if (this.metodo === "newton") {
        axios
          .post("http://127.0.0.1:5000/api/newton", requerimiento2, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta
          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else if (this.metodo === "puntofijo") {
        axios
          .post("http://127.0.0.1:5000/api/puntofijo", requerimiento3, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta
          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else if (this.metodo === "secante") {
        axios
          .post("http://127.0.0.1:5000/api/secante", requerimiento4, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta
          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else if (this.metodo === "simpson") {
        axios
          .post("http://127.0.0.1:5000/api/simpson", requerimiento5, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta
          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else if (this.metodo === "trapezoidal") {
        axios
          .post("http://127.0.0.1:5000/api/trapezoidal", requerimiento5, {
            headers: {
              "Content-Type": "application/json",
            },
          })
          .then((response) => {
            this.resultado = response.data.response;
            console.log(response.data); // Acceder a los datos de la respuesta
          })
          .catch((error) => {
            console.error("Error al obtener datos de la API:", error);
          });
      } else {
        console.error("Método no válido");
        return;
      }
    }, //agregar nuevas funciones
    borrarUltimoCaracter() {
      // Verifica si la ecuación no está vacía antes de borrar
      if (this.ecuacion.length > 0) {
        this.ecuacion = this.ecuacion.slice(0, -1); // Elimina el último carácter
      }
    },
  },
  mounted() {},
};
</script>


<style scoped>
/* Estilos para el componente Calculator-component */
/* Fondo para todo el componente */
.calculator-container {
  background-color: #cccbd8;
}

/* Estilos para los botones */
.calculator-button {
  width: 45px;
  height: 45px;
  font-size: 1rem;
  font-family: Arial, sans-serif; /* Cambia la fuente aquí */
  background-color: #ffffff; /* Color de fondo */
  color: #020202; /* Color del texto */
  border: 1px solid #ffffff; /* Borde */
  border-radius: 5px; /* Bordes redondeados */
  margin: 5px;
  cursor: pointer; /* Cambia el cursor al pasar sobre el botón */
  transition: background-color 0.3s ease; /* Transición suave del color de fondo */
}

.calculator-buttonL {
  width: 40px;
  height: 20px;
  font-size: 0.9rem;
  font-weight: bold;
  background-color: #ffffff; /* Color de fondo */
  color: #020202; /* Color del texto */
  border: 1px solid #c1b9b9; /* Borde */
  border-radius: 5px; /* Bordes redondeados */
  margin: 5px;
  cursor: pointer; /* Cambia el cursor al pasar sobre el botón */
  transition: background-color 0.3s ease; /* Transición suave del color de fondo */
}

.calculator-button:hover {
  background-color: #abb0ab; /* Cambia el color de fondo al pasar el mouse */
}

/* Estilos para el cuadro de resultado */
.calculator-result {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  text-align: right;
  font-size: 1.2rem;
  margin-top: 10px;
}

.centered-content {
  align-items: center;
  justify-content: center;
  height: 6vh;
}
</style>