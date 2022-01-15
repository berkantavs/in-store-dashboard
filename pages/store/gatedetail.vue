<template>
  <div>
    <div class="p-5 text-center">
      <img src="@/assets/images/logo.png" width="180" alt="" />
      <h5 class="d-block mt-2 mb-3">Gates Analytics</h5>
      <div class="mt-4">
        <b-row v-if="is_ready">
          <b-col lg="7">
            <b-card no-body class="rounded border shadow bg-light">
              <apexchart
                type="line"
                :options="chartOptions"
                :series="series"
              ></apexchart>
            </b-card>
            <b-card no-body class="mt-3 rounded border shadow bg-light">
              <apexchart
                type="area"
                height="350"
                :options="areaOptions"
                :series="series"
              ></apexchart>
            </b-card>
          </b-col>
          <b-col lg="5">
            <b-card no-body class="rounded border shadow bg-light">
              <apexchart
                type="bar"
                height="250"
                :options="radialBarOptions"
                :series="series"
              ></apexchart>
            </b-card>
            <b-card no-body class="mt-3 rounded border shadow bg-light">
              <apexchart
                type="bar"
                height="250"
                :options="barOptions"
                :series="series"
              ></apexchart>
            </b-card>
            </b-col>
        </b-row>
      </div>
    </div>
  </div>
</template>

<script>
import apexchart from 'vue-apexcharts'
export default {
  components: { apexchart },
  data() {
    return {
      is_ready: false,

      gate_report: null,
      chartOptions: {
        chart: {
          width: '100%',
          type: 'line',
        },
        stroke: {
          curve: 'smooth',
        },
        fill: {
          type: 'solid',
          opacity: [0.35, 1],
        },
        labels: [
          '11/10/2021 9:00',
          '11/10/2021 9:30',
          '11/10/2021 10:00',
          '11/10/2021 10:30',
          '11/10/2021 11:00',
          '11/10/2021 11:30',
          '11/10/2021 12:00',
        ],
        markers: {
          size: 0,
        },
        yaxis: [
          {
            title: {
              text: '',
            },
          },
          {
            opposite: true,
            title: {
              text: '',
            },
          },
        ],
      },
      series: [
        {
          name: 'yaz_reyon',
          data: [4.6, 9.25, 4.6, 3.5, 10.16, 7.23],
        },
        {
          name: 'erkek_reyon',
          data: [0.13, 0.03, 0.13, 3.08, 7.03, 3.48],
        },
        {
          name: 'kadin_reyon',
          data: [0.58, 2.13, 0.58, 0.9, 2.45, 3.18],
        },
        {
          name: 'bebek_reyon',
          data: [0.03, 0, 0.03, 1.18, 5.63, 0],
        },
      ],
      radialBarOptions: {
        chart: {
          type: 'bar',
          height: 250,
          stacked: true,
        },
        plotOptions: {
          bar: {
            horizontal: true,
          },
        },
        stroke: {
          width: 1,
          colors: ['#fff'],
        },

        xaxis: {
          categories: [
            '11/10/2021 9:00',
            '11/10/2021 9:30',
            '11/10/2021 10:00',
            '11/10/2021 10:30',
            '11/10/2021 11:00',
            '11/10/2021 11:30',
            '11/10/2021 12:00',
          ],
          labels: {
            formatter: function (val) {
              return val
            },
          },
        },
        yaxis: {
          title: {
            text: undefined,
          },
        },
        tooltip: {
          y: {
            formatter: function (val) {
              return val
            },
          },
        },
        fill: {
          opacity: 1,
        },
        legend: {
          position: 'top',
          horizontalAlign: 'left',
          offsetX: 40,
        },
      },
      barOptions: {
        chart: {
          height: 250,
          type: 'bar',
        },
        plotOptions: {
          bar: {
            borderRadius: 10,
            dataLabels: {
              position: 'top', // top, center, bottom
            },
          },
        },
        dataLabels: {
          enabled: true,
          formatter: function (val) {
            return val
          },
          offsetY: -20,
          style: {
            fontSize: '12px',
            colors: ['#304758'],
          },
        },

        xaxis: {
          categories: [
            '11/10/2021 9:00',
            '11/10/2021 9:30',
            '11/10/2021 10:00',
            '11/10/2021 10:30',
            '11/10/2021 11:00',
            '11/10/2021 11:30',
            '11/10/2021 12:00',
          ],
          position: 'top',
          axisBorder: {
            show: false,
          },
          axisTicks: {
            show: false,
          },
          crosshairs: {
            fill: {
              type: 'gradient',
              gradient: {
                colorFrom: '#D8E3F0',
                colorTo: '#BED1E6',
                stops: [0, 100],
                opacityFrom: 0.4,
                opacityTo: 0.5,
              },
            },
          },
          tooltip: {
            enabled: true,
          },
        },
        yaxis: {
          axisBorder: {
            show: false,
          },
          axisTicks: {
            show: false,
          },
          labels: {
            show: false,
            formatter: function (val) {
              return val
            },
          },
        },
      },
      areaOptions: {
        chart: {
          height: 350,
          type: 'area',
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: 'smooth',
        },
        xaxis: {
          type: 'datetime',
          categories: [
            '11/10/2021 9:00',
            '11/10/2021 9:30',
            '11/10/2021 10:00',
            '11/10/2021 10:30',
            '11/10/2021 11:00',
            '11/10/2021 11:30',
            '11/10/2021 12:00',
          ],
        },
        tooltip: {
          x: {
            format: 'dd/MM/yy HH:mm',
          },
        },
      },
      raidOptions: {
        chart: {
          height: 390,
          type: 'radialBar',
        },
        plotOptions: {
          radialBar: {
            offsetY: 0,
            startAngle: 0,
            endAngle: 270,
            hollow: {
              margin: 5,
              size: '30%',
              background: 'transparent',
              image: undefined,
            },
            dataLabels: {
              name: {
                show: false,
              },
              value: {
                show: false,
              },
            },
          },
        },
        colors: ['#1ab7ea', '#0084ff', '#39539E', '#0077B5'],
        labels: [
          '11/10/2021 9:00',
          '11/10/2021 9:30',
          '11/10/2021 10:00',
          '11/10/2021 10:30',
          '11/10/2021 11:00',
          '11/10/2021 11:30',
          '11/10/2021 12:00',
        ],
        legend: {
          show: true,
          floating: true,
          fontSize: '16px',
          position: 'left',
          offsetX: 160,
          offsetY: 15,
          labels: {
            useSeriesColors: true,
          },
          markers: {
            size: 0,
          },
          itemMargin: {
            vertical: 3,
          },
        },
        responsive: [
          {
            breakpoint: 480,
            options: {
              legend: {
                show: false,
              },
            },
          },
        ],
      },
    }
  },
  async created() {
    if (this.$route.query.id) {
      this.getGateInfo(this.$route.query.id)
    }
  },
  methods: {
    async getGateInfo(gate_id) {
      try {
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/gates/' + gate_id
        )
        if (response && response.data && response.data.reports) {
          this.gate_report = response.data.reports
          this.is_ready = true
        }
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style></style>
