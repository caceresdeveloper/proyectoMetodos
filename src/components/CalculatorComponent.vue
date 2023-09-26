<template>
  <div class="calculator">
    <h1 class="calculator-title">{{ titulo }}</h1>
    <div class="calculator-screen">
      <input type="text" v-model="ecuacion" @input="evaluateExpression" />
    </div>
    <div class="calculator-buttons">
      <button
        v-for="button in buttons"
        :key="button"
        @click="handleButtonClick(button)"
      >
        {{ button }}
      </button>
    </div>
    <br />
    <div class="calculator-result">
      <span>{{ resultado }}</span>
    </div>
    <br />

    <div class="calculator-options">
      <div v-if="typeMetod === 1" class="option-group">
        <label for="a">a</label>
        <input v-model="a" type="number" class="option-input" />
        <label for="b">b</label>
        <input v-model="b" type="number" class="option-input" />
      </div>
      <div v-if="typeMetod === 3" class="option-group">
        <label for="xi">xi</label>
        <input v-model="xi" type="number" class="option-input" />
      </div>
      <div v-if="typeMetod === 5" class="option-group">
        <label for="x0">x0</label>
        <input v-model="x0" type="number" class="option-input" />
        <label for="x1">x1</label>
        <input v-model="x1" type="number" class="option-input" />
      </div>
      <div v-if="typeMetod === 6" class="option-group">
        <label for="inferior">inf</label>
        <input v-model="inferior" type="number" class="option-input" />
        <label for="superior">sup</label>
        <input v-model="superior" type="number" class="option-input" />
        <label for="intervalos">intervalos</label>
        <input v-model="intervalos" type="number" class="option-input" />
      </div>
      <div v-if="typeMetod === 7" class="option-group">
        <label for="inferior">inf</label>
        <input v-model="inferior" type="number" class="option-input" />
        <label for="superior">sup</label>
        <input v-model="superior" type="number" class="option-input" />
        <label for="intervalos">intervalos</label>
        <input v-model="intervalos" type="number" class="option-input" />
      </div>
      <br />
      <div>
        <button @click="enviar" class="custom-button">Enviar</button>
      </div>
      <br />
    </div>
  </div>
  <vue-spinner :isLoading="loading" :color="'#000'" :size="'md'"></vue-spinner>
  <div v-if="grafistatus === 1">
    <grafica :functionString="functionS" :points="point" />
  </div>
</template>

<script>
import grafica from "./GraphFuncion.vue";
import axios from "axios";
import Swal from "sweetalert2";
import VueSpinner from 'vue-spinner/src/PulseLoader.vue'; 

export default {
  props: {
    titulo: String,
    typeMetod: Number,
  },
  data() {
    return {
      loading: false,
      grafistatus: 0,
      functionS: "",
      point: [],
      a: 0,
      b: 0,
      xi: 0,
      x0: 0,
      x1: 0,
      inferior: 0,
      superior: 0,
      intervalos: 0,
      resultado: 0,

      ecuacion: "",
      buttons: [
        "1",
        "2",
        "3",
        "+",
        "4",
        "5",
        "6",
        "-",
        "7",
        "8",
        "9",
        "*",
        "(",
        ")",
        "/",
        "sin",
        "cos",
        "tan",
        "sqrt",
        "^",
        "C",
        "0",
        "x",
        "=",
      ],
    };
  },
  components: {
    grafica,
    VueSpinner,
  },
  methods: {
    handleButtonClick(button) {
      if (button === "C") {
        this.ecuacion = this.ecuacion.slice(0, -1);
      } else {
        this.ecuacion += button;
      }
    },
    evaluateExpression() {},

    async postData(url, data) {
      try {
        const response = await axios.post(url, data, {
          headers: {
            "Content-Type": "application/json",
          },
        });
        return response.data.response;
      } catch (error) {
        console.error("Error al obtener datos de la API:", error);
        Swal.fire({
          icon: "error",
          title: "Algo salió mal",
          text: "El método diverge o ingresaste los datos de manera incorrecta. Por favor, verifica la información que has introducido.",
        });
        throw error;
      }
    },

    async llamarAPI() {
      try {
        let requerimiento;
        const apiUrl = "http://127.0.0.1:5000/api/";
        let metodo = "";
        switch (this.typeMetod) {
          case 1:
            requerimiento = { ecuacion: this.ecuacion, a: this.a, b: this.b };
            return await this.postData(apiUrl + "biseccion", requerimiento);
          case 3:
            requerimiento = { ecuacion: this.ecuacion, xi: this.xi };
            return await this.postData(apiUrl + "newton", requerimiento);
          case 4:
            requerimiento = { ecuacion: this.ecuacion };
            return await this.postData(apiUrl + "puntofijo", requerimiento);
          case 5:
            requerimiento = {
              ecuacion: this.ecuacion,
              x0: this.x0,
              x1: this.x1,
            };
            return await this.postData(apiUrl + "secante", requerimiento);
          case 6:
          case 7:
            requerimiento = {
              ecuacion: this.ecuacion,
              inferior: this.inferior,
              superior: this.superior,
              intervalos: this.intervalos,
            };
            metodo = this.typeMetod == 6 ? "simpson" : "trapezoidal";

            return await this.postData(apiUrl + metodo, requerimiento);
          default:
            console.error("Método no válido");
            return;
        }
      } catch (error) {
        console.error("Error al obtener datos de la API:", error);
        Swal.fire({
          icon: "error",
          title: "Algo salió mal",
          text: "El método diverge o ingresaste los datos de manera incorrecta. Por favor, verifica la información que has introducido.",
        });
        throw error;
      }
    },

    async enviar() {
      this.grafistatus = 0;
      try {
        this.loading = true;
        this.resultado = await this.llamarAPI.call(this);
        this.functionS = this.ecuacion;
        if (this.resultado) {
          if (Array.isArray(this.resultado)) {
            this.resultado.forEach((element) => {
              let aux = element + "";
              let pointaux = {
                x: parseFloat(aux.replace(".", ",")),
                y: 0,
              };
              this.point.push(pointaux);
            });
          } else {
            this.resultado = this.resultado + "";
            let pointaux = {
              x: parseFloat(this.resultado.replace(".", ",")),
              y: 0,
            };
            this.point.push(pointaux);
          }
        }
        this.grafistatus = 1;
        this.loading = false;
      } catch (error) {
        Swal.fire({
          icon: "error",
          title: "Algo salió mal",
          text: "El método diverge o ingresaste los datos de manera incorrecta. Por favor, verifica la información que has introducido.",
        });
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.custom-button {
  background-color: #ff5722;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.custom-button:hover {
  background-color: #ff7043;
}

.option-input {
  padding: 8px;
  margin: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
  width: 100px;
}

.calculator-options {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.option-group {
  display: flex;
  align-items: center;
  margin-top: 10px;
}

.option-group label {
  font-weight: bold;
  margin-right: 5px;
}

.calculator-result {
  background-color: #4caf50;
  color: #fff;
  text-align: center;
  font-size: 28px;
  padding: 10px;
  margin-top: 20px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
}

.calculator {
  width: 300px;
  margin: 0 auto;
}

.calculator-title {
  font-family: "Arial", sans-serif;
  font-size: 28px;
  color: #3498db;
  margin-bottom: 20px;
  text-transform: uppercase;
  text-align: center;
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2);
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
