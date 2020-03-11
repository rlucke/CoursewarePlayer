<template>
  <div class="ChartBlock">
      <canvas class="cw-chartblock-canvas" ref="chartCanvas" />
  </div>
</template>

<script>
import Chart from 'chart.js'

export default {
    name: 'ChartBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data(){
        return{
            colors: {
                'red': '192, 57, 43',
                'blue': '52, 152, 219',
                'yellow': '241, 196, 15',
                'green': '46, 204, 113',
                'purple': '155, 89, 182',
                'orange': '230, 126, 34',
                'turquoise': '26, 188, 156',
                'grey': '52, 73, 94',
                'lightgrey': '149, 165, 166',
                'black': '0, 0, 0'
            }
        }
    },
    mounted () {
        this.buildChart();
    },
    methods: {
        buildChart() {
            let ctx = this.$refs.chartCanvas.getContext('2d');
            let type = this.block.data.chart_type;
            let label = this.block.data.chart_label;
            let labels = [];
            let data = [];
            let backgroundColor = [];
            let borderColor = [];

            this.block.data.chart_content.forEach( item => {
                labels.push(item.label);
                data.push(item.value);
                backgroundColor.push('rgba('+this.colors[item.color]+', 0.6)');
                borderColor.push('rgba('+this.colors[item.color]+', 1.0)');
            });

            switch (type) {
                case 'bar':
                case 'horizontalBar':
                    new Chart(ctx, {
                        type: type,
                        data: {
                            labels: labels,
                            datasets: [{
                                label: label,
                                data: data,
                                backgroundColor: backgroundColor,
                                borderColor: borderColor,
                                borderWidth: 1
                            }]
                        },
                        options: {
                            scales: {
                                yAxes: [{
                                    ticks: {
                                        beginAtZero:true
                                    }
                                }],
                                xAxes: [{
                                    ticks: {
                                        beginAtZero:true
                                    }
                                }]
                            },
                            legend: {
                                display: false
                            },
                            title:{
                                display: true,
                                text: label
                            }
                        }
                    });
                    break;
                case 'pie':
                case 'doughnut':
                case 'polarArea':
                    new Chart(ctx, {
                        type: type,
                        data: {
                            labels: labels,
                            datasets: [{
                                data: data,
                                backgroundColor: backgroundColor,
                                borderWidth: 1
                            }]
                        },
                        options: {
                            title:{
                                display: true,
                                text: label
                            }
                        }
                    });
                    break;
                case 'line':
                    new Chart(ctx, {
                        type: type,
                        data: {
                            labels: labels,
                            datasets: [{
                                label: label,
                                data: data,
                                fill: false, 
                                borderWidth: 2,
                                pointBackgroundColor: borderColor
                            }]
                        },
                        options: {
                            title:{
                                display: true,
                                text: label
                            }
                        }
                    });
                    break;
            }
        }
    }
}
</script>