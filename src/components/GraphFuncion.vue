<template>
  <div>
    <canvas ref="chart" :style="{ height: '1000px', width: '400px' }"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
import * as math from "mathjs";

export default {
  props: {
    functionString: String,
    points: Array, // Nueva prop para los puntos [{x, y}]
  },
  data() {
    return {
      chartInstance: null,
      minX: -100,
      maxX: 100,
      minY: -100,
      maxY: 100,
      step: 1,
    };
  },
  mounted() {
    this.drawChart();
  },
  watch: {
    functionString: "drawChart",
    points: "drawChart", // Vuelve a dibujar la gráfica cuando cambian los puntos
  },
  methods: {
    drawChart() {
      if (!this.functionString) {
        return;
      }

      try {
        const parsedFunction = math.compile(this.functionString);
        const labels = [];
        const data = [];

        for (let x = this.minX; x <= this.maxX; x += this.step) {
          const y = parsedFunction.evaluate({ x });
          labels.push(x);
          data.push(y);
        }

        if (!this.chartInstance) {
          this.chartInstance = new Chart(this.$refs.chart, {
            type: "line",
            data: {
              labels,
              datasets: [
                {
                  label: "Function",
                  data,
                  borderColor: "blue",
                },
                {
                  label: "Points", // Etiqueta para los puntos
                  data: this.points, // Agregar los puntos a los datos
                  pointRadius: 5, // Tamaño de los puntos
                  pointBackgroundColor: "red", // Color de los puntos
                  showLine: false, // No conectar los puntos con líneas
                },
              ],
            },
            options: {
              responsive: true,
              maintainAspectRatio: false,
              scales: {
                x: {
                  min: this.minX,
                  max: this.maxX,
                  stepSize: this.step,
                },
                y: {
                  min: this.minY,
                  max: this.maxY,
                  stepSize: this.step,
                },
              },
            },
          });
        } else {
          // Actualizar los datos y volver a dibujar
          this.chartInstance.data.labels = labels;
          this.chartInstance.data.datasets[0].data = data;
          this.chartInstance.data.datasets[1].data = this.points;
          this.chartInstance.update();
        }
      } catch (error) {
        console.error("Error al analizar la función:", error);
      }
    },
  },
};
</script>
