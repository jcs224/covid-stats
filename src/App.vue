<template>
  <div class="flex flex-col h-screen">
    <h1 class="bg-green-500 text-white text-center text-4xl">COVID-19 Stats</h1>
    <div class="p-3 bg-gray-200 flex-1">
      <canvas class="bg-white" id="chart1"></canvas>
    </div>
  </div>
</template>

<script>
import 'chart.js'

export default {
  mounted() {
    console.log(document.getElementById('chart1'))
    let chartOne = new Chart(document.getElementById('chart1'), {
      type: 'bar',
      data: {
        labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
        datasets: [{
            label: '# of Votes',
            data: [12, 19, 3, 5, 2, 3],
            backgroundColor: [
                'rgba(255, 99, 132, 0.2)',
                'rgba(54, 162, 235, 0.2)',
                'rgba(255, 206, 86, 0.2)',
                'rgba(75, 192, 192, 0.2)',
                'rgba(153, 102, 255, 0.2)',
                'rgba(255, 159, 64, 0.2)'
            ],
            borderColor: [
                'rgba(255, 99, 132, 1)',
                'rgba(54, 162, 235, 1)',
                'rgba(255, 206, 86, 1)',
                'rgba(75, 192, 192, 1)',
                'rgba(153, 102, 255, 1)',
                'rgba(255, 159, 64, 1)'
            ],
            borderWidth: 1
        }]
    },
    })
  }
}
</script>
