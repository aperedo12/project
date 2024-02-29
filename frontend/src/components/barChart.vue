<template>
  <div>
    <canvas class="p-10" ref="attendanceChart"></canvas>
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
    const attendanceChartRef = ref(null);

    onMounted(() => {
      // Register chart components before using
      Chart.register(...registerables);

      // Initialize the chart
      const attendanceChart = new Chart(attendanceChartRef.value, {
        type: 'bar',
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
          scales: {
            y: {
              ticks: {
                stepSize: 1
              }
            },
            x: {
              gridLines: {
                display: false
              }
            }
          },
          plugins: {
            legend: {
              display: false
            },
            title: {
              display: true,
              text: 'Attendance Chart'
            }
          },
          responsive: true,
          maintainAspectRatio: true
        }
      });

      // Cleanup function to destroy chart when the component unmounts
      return () => {
        if (attendanceChart) {
          attendanceChart.destroy();
        }
      };
    });

    return {
      attendanceChartRef
    };
  }
};
</script>
