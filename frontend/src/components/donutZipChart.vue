<template>
  <div class="w-1/2">
    <canvas class="p-10" ref="zipChart"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';
import { onMounted, ref } from 'vue';

export default {
  props: {
    label: Array,
    chartData: Array
  },
  setup(props) {
    const zipChartRef = ref(null);

    onMounted(() => {
      Chart.register(...registerables);

      const zipChart = new Chart(zipChartRef.value, {
        type: 'doughnut',
        data: {
          labels: props.label,
          datasets: [
            {
              borderWidth: 1,
              data: props.chartData
            }
          ]
        },
        options: {
          plugins: {
            legend: {
              display: false
            },
            title: {
              display: true,
              text: 'Clients by Zip Code'
            }
          },
          responsive: true,
          maintainAspectRatio: true
        }
      });

      return () => {
        if (zipChart) {
          zipChart.destroy();
        }
      };
    });

    return {
      zipChartRef
    };
  }
};
</script>
