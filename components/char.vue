<script setup lang="ts">
import chart from "chart.js/auto";

const labels = ["Januari", "Februari", "March", "April", "May", "June"];

const data = {
  labels: labels,
  datasets: [
    {
      label: "pengunjung",
      data: [6, 8, 10, 9, 9, 3],
      backgroundColor: [
        "rgb(220, 220, 220)",
        "rgb(220, 220, 220)",
        "rgb(220, 220, 220)",
        "rgb(220, 220, 220)",
        "rgb(220, 220, 220)",
        "rgb(220, 220, 220)",
      ],
    },
  ],
};

const config = {
  type: "bar",
  data: data,
  options: {},
};

onMounted(() => {
  const myChart = new chart(document.getElementById("myChart"), config);
});
</script>

<template>
  <div>
    <canvas id="myChart"></canvas>
  </div>
</template> 
