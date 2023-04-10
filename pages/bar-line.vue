<template>
  <div>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { Chart, LineController, BarController, CategoryScale, LinearScale } from 'chart.js';

Chart.register(LineController, BarController, CategoryScale, LinearScale);

let delayed;
export default {
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    const self = this;
    const canvas = self.$refs.canvas;
    const ctx = canvas.getContext('2d');
    self.chart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        datasets: [
          {
            type: 'line',
            label: 'Line Dataset',
            data: [20, 50, 30, 40, 50, 60, 50, 30, 70, 50, 35, 70],
            borderColor: '#91cc75',
            fill: false,
            tension: 0.3
          },
          {
            type: 'bar',
            label: 'Bar Dataset',
            data: [30, 80, 40, 70, 10, 80, 30, 65, 75, 20, 45, 60],
            backgroundColor: '#5470c6',
          },
        ],
      },
      options: {
        animation: {
          onComplete: () => {
            delayed = true;
          },
          delay: (context) => {
            let delay = 0;
            if (context.type === 'data' && context.mode === 'default' && !delayed) {
              delay = context.dataIndex * 300 + context.datasetIndex * 100;
            }
            return delay;
          },
          // tension: {
          //   duration: 1000,
          //   easing: 'linear',
          //   from: 1,
          //   to: 0,
          //   loop: true
          // }
        },
        scales: {
          x: {
            stacked: true,
          },
          y: {
            stacked: true
          }
        }
      }
    });
  },
  beforeDestroy() {
    self.chart.destroy();
  },
};
</script>