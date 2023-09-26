<template>
  <div class="matrix-generator">
    <h1 class="calculator-title">{{ titulo }}</h1>
    <br />
    <div>
      <label for="n-input">Ingrese un número (n):</label>
      <input
        id="n-input"
        v-model="n"
        type="number"
        @input="validateInput"
        class="input-number"
      />
    </div>
    <div v-if="matrixGenerated" class="matrix-container">
      <label class="matrix-row">
        <span
          v-for="(cell, colIndex) in matrix[0]"
          :key="colIndex"
          :style="{ margin: margenExistente + n + 'px' }"
        >
          x{{ colIndex + 1 }}
        </span>
      </label>
      <label
        v-for="(row, rowIndex) in matrix"
        :key="rowIndex"
        class="matrix-row"
      >
        <span class="margined-span"> E{{ rowIndex + 1 }} </span>
        <span
          v-for="(cell, colIndex) in row"
          :key="colIndex"
          class="matrix-cell"
        >
          <input
            v-model="matrix[rowIndex][colIndex]"
            type="text"
            class="input-field"
            @input="validateMatrixInput(rowIndex, colIndex)"
          />
        </span>
      </label>
    </div>
  </div>
  <div class="results-container">
    <div v-for="(result, index) in results" :key="index" class="result-input">
      <label for="result-input-{{ index }}">Ecuacion {{ index + 1 }} =</label>
      <input
        v-model="results[index]"
        :id="'result-input-' + index"
        type="text"
        class="input-field"
      />
    </div>
    <br />
    <div>
      <button v-if="matrixGenerated" @click="llamarAPI" class="custom-button">
        Enviar
      </button>
    </div>
    <br />
    <div>
      <br />
      <div v-if="resultadoMetodo != ''" class="calculator-result">
        <span>{{ resultadoMetodo }}</span>
      </div>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from 'sweetalert2';

export default {
  props: {
    titulo: String,
    typeMetod: Number,
  },
  data() {
    return {
      resultadoMetodo: "",
      margenExistente: 40,
      n: null,
      matrix: null,
      results: null,
      matrixGenerated: false,
    };
  },
  methods: {
    validateInput() {
      if (this.n < 0) {
        this.n = Math.abs(this.n);
      }

      this.generateMatrix();
    },

    generateMatrix() {
      this.matrix = new Array(this.n)
        .fill([])
        .map(() => new Array(this.n).fill(0));
      this.results = new Array(this.n).fill(0);
      this.matrixGenerated = true;
    },

    validateMatrixInput(rowIndex, colIndex) {
      console.log(this.matrix);

      const inputValue = this.matrix[rowIndex][colIndex];
      const validInputPattern = /^[0-9+\-*/().\s]+$/;

      if (!validInputPattern.test(inputValue)) {
        this.matrix[rowIndex][colIndex] = "";
      }
    },

    async postData(url, data) {
      try {
        const response = await axios.post(url, data, {
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.resultadoMetodo = response.data.response;
        return response.data.response;
      } catch (error) {
        console.error("Error al obtener datos de la API:", error);
        Swal.fire({
          icon: 'error',
          title: 'Algo salió mal',
          text: 'El método diverge o ingresaste los datos de manera incorrecta. Por favor, verifica la información que has introducido.',
        });
        throw error;
      }
    },

    async llamarAPI() {
      try {
        let requerimiento;
        const apiUrl = "http://127.0.0.1:5000/api/";
        let metodo = "";

        requerimiento = {
          ecuacion: this.matrix,
          resultados: this.results,
          xi: new Array(this.n).fill(0),
        };
        metodo = this.typeMetod == 2 ? "jacobi" : "sedial";

        return await this.postData(apiUrl + metodo, requerimiento);
      } catch (error) {
        console.log("error");
        Swal.fire({
          icon: 'error',
          title: 'Algo salió mal',
          text: 'El método diverge o ingresaste los datos de manera incorrecta. Por favor, verifica la información que has introducido.',
        });
      }
    },
  },
};
</script>
<style scoped>
.calculator-title {
  font-family: "Arial", sans-serif;
  font-size: 28px;
  color: #3498db;
  margin-bottom: 20px;
  text-transform: uppercase;
  text-align: center;
  text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2);
}

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

.calculator-result {
  background-color: #4caf50;
  color: #fff;
  text-align: center;
  font-size: 28px;
  padding: 10px;
  margin-top: 20px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
}

.input-container,
.results-container {
  text-align: center;
}
.margined-span {
  margin: 20px;
}

.matrix-generator {
  text-align: center;
  padding: 20px;
}

.input-number {
  width: 50px;
  padding: 5px;
  margin: 10px;
}

.matrix-container {
  margin-top: 20px;
  display: inline-block;
}

.matrix-row {
  display: block;
  margin-bottom: 5px;
}

.matrix-cell {
  display: inline-block;
  margin-right: 5px;
}

.input-field {
  border: 1px solid #ccc;
  padding: 5px;
  width: 100px;
}

.submit-button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #0056b3;
}
</style>
