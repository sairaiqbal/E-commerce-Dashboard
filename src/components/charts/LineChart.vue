<template>
  <canvas ref="canvas" />
</template>

<script setup>
import { ref, watch, onMounted } from "vue";
import {
  Chart,
  LineController,
  LineElement,
  PointElement,
  LinearScale,
  Title,
  CategoryScale,
  TimeScale,
  Tooltip,
  Legend,
} from "chart.js";

import "chartjs-adapter-date-fns"; // date adapter

Chart.register(
  LineController,
  LineElement,
  PointElement,
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
    type: "line",
    data: {
      labels: data.map((item) => new Date(item.date)),
      datasets: [
        {
          label: "Sales",
          data: data.map((item) => item.value),
          borderColor: "#7c3aed",
          backgroundColor: "rgba(124, 58, 237, 0.3)",
          fill: true,
          tension: 0.3,
          pointRadius: 4,
          pointHoverRadius: 6,
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
            tooltipFormat: "PPP", // e.g. May 1, 2025
            displayFormats: {
              day: "MMM d", // e.g. May 1
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
