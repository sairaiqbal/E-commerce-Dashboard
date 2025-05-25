<template>
  <canvas ref="canvas" />
</template>

<script setup>
import { ref, watch, onMounted } from "vue";
import {
  Chart,
  BarController,
  BarElement,
  LinearScale,
  Title,
  CategoryScale,
  TimeScale,
  Tooltip,
  Legend,
} from "chart.js";

import "chartjs-adapter-date-fns";

Chart.register(
  BarController,
  BarElement,
  LinearScale,
  Title,
  CategoryScale,
  TimeScale,
  Tooltip,
  Legend
);

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
});

const canvas = ref(null);
let chartInstance = null;

function createChart(data) {
  if (chartInstance) chartInstance.destroy();

  chartInstance = new Chart(canvas.value, {
    type: "bar",
    data: {
      labels: data.map((item) => new Date(item.date)),
      datasets: [
        {
          label: "Orders",
          data: data.map((item) => item.value),
          backgroundColor: "#7c3aed",
        },
      ],
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true },
        tooltip: { enabled: true },
      },
      scales: {
        x: {
          type: "time",
          time: {
            unit: "day",
            tooltipFormat: "PPP",
            displayFormats: {
              day: "MMM d",
            },
          },
          ticks: {
            maxRotation: 45,
            minRotation: 30,
            autoSkip: true,
            maxTicksLimit: 7,
          },
          title: {
            display: true,
            text: "Date",
          },
        },
        y: {
          beginAtZero: true,
          title: {
            display: true,
            text: "Value",
          },
        },
      },
    },
  });
}

onMounted(() => {
  createChart(props.data);
});

watch(
  () => props.data,
  (newData) => {
    createChart(newData);
  }
);
</script>
