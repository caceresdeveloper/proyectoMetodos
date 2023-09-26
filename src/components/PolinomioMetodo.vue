<template>
  <div class="centered">
    <label for="numberInput">Ingresa un número:</label>
    <input
      id="numberInput"
      type="number"
      v-model="inputNumber"
      @input="updateArrays"
      class="input-box"
    />
    <div class="arrays-container">
      <div class="array">
        <p>Valores en X:</p>
        <ul>
          <li v-for="(value, index) in arrayX" :key="index">
            <input type="number" v-model="arrayX[index]" class="array-input" />
          </li>
        </ul>
      </div>
      <div class="array">
        <p>Valores en Y:</p>
        <ul>
          <li v-for="(value, index) in arrayY" :key="index">
            <input type="number" v-model="arrayY[index]" class="array-input" />
          </li>
        </ul>
      </div>
    </div>

    <div v-if="inputNumber > 0">
      <div class="calculator-result">
        <span>{{ resultado }}</span>
      </div>
      <br />
      <button @click="enviar" class="custom-button">Enviar</button>
    </div>
  </div>

  <div v-if="grafistatus === 1">
    <grafica :functionString="functionS" />
  </div>
  
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
import grafica from "./GraphFuncion.vue";

export default {
  data() {
    return {
      functionS:"",
      grafistatus: 0,
      resultado:"",
      inputNumber: 0,
      arrayX: [],
      arrayY: [],
    };
  },
  components:{
    grafica,
  },
  methods: {
    updateArrays() {
      const num = parseInt(this.inputNumber);
      this.arrayX = Array(num).fill(0);
      this.arrayY = Array(num).fill(0);
    },

    async enviar() {
      this.grafistatus =0
      try {
        const apiUrl = "http://127.0.0.1:5000/api/polinomio";
        let requerimiento = {
          valoresx: this.arrayX,
          valoresy: this.arrayY,
        };

        const response = await axios.post(apiUrl, requerimiento, {
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.resultado= response.data.response;
        this.functionS = response.data.response.replace("**", "^");
        this.grafistatus =1
      } catch (error) {
        console.error("Error al obtener datos de la API:", error);
        Swal.fire({
          icon: "error",
          title: "Algo salió mal",
          text: "Por favor, revisa los datos que ingresaste.",
        });
        throw error;
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
.centered {
  text-align: center;
  margin-top: 50px;
}

.input-box {
  width: 100px;
  padding: 10px;
  margin: 10px;
  border: 1px solid #ccc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}

.arrays-container {
  display: flex;
  justify-content: center;
}

.array {
  margin: 20px;
  text-align: left;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 10px;
  border-radius: 4px;
}

.array-input {
  width: 40px;
  margin-right: 5px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 5px 0;
}
</style>
