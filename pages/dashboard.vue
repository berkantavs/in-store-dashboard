<template>
  <client-only>
    <b-overlay :show="report_overlay" rounded="sm" opacity="1">
      <div>
        <div>
          <b-row no-gutters>
            <b-col lg="6" class="text-left">
              <b-row>
                <b-col>
                  <b-form-datepicker
                    class=" bg-light"
                    placeholder="Start Date"
                    v-model="start_date"
                  ></b-form-datepicker>
                </b-col>
                <b-col>
                  <b-form-datepicker
                    class=" bg-light"
                    placeholder="End Date"
                    v-model="end_date"
                  ></b-form-datepicker>
                </b-col>
              </b-row>
            </b-col>
            <b-col lg="6"></b-col>
          </b-row>
          <div class="text-center mt-3">
            <h5>
              <b> MAĞAZA GİRİŞ ÇIKIŞ ANALİZLERİ </b>
            </h5>
          </div>
          <div class="mt-3">
            <b-row v-if="is_ready">
              <b-col lg="12" class="pl-0">
                <b-card no-body class="rounded border shadow p-2">
                  <apexchart
                    type="bar"
                    height="400"
                    :options="barChartOptions"
                    :series="series"
                  ></apexchart>
                </b-card>
                <div class="text-center">
                  <h5 class="mb-4 mt-3">
                    <b> REYON ZİYARET ANALİZLERİ </b>
                  </h5>
                </div>
                <div>
                  <b-row>
                    <b-col cols="12" lg="6">
                      <b-card no-body class="rounded border shadow p-2">
                        <apexchart
                          type="bar"
                          height="350"
                          :options="radialBarOptions"
                          :series="series"
                        ></apexchart>
                      </b-card>
                    </b-col>
                    <b-col cols="12" lg="6">
                      <b-card no-body class="mt-3 rounded border shadow p-2">
                        <apexchart
                          type="donut"
                          height="380"
                          :options="barOptions"
                          :series="barOptions.series"
                        ></apexchart>
                      </b-card>
                    </b-col>
                  </b-row>
                </div>
                <div class="mt-2 mb-4 text-center">
                  <h5>
                    <b>KASA YOĞUNLUĞU ANALİZLERİ</b>
                  </h5>
                </div>
                <b-card no-body class="mt-3 mb-3 rounded border shadow p-2">
                  <apexchart
                    type="area"
                    height="400"
                    :options="areaOptions"
                    :series="series"
                  ></apexchart>
                </b-card>
              </b-col>
              <!-- <b-col lg="5">
              
            </b-col> -->
            </b-row>
          </div>
        </div>
      </div>
    </b-overlay>
  </client-only>
</template>

<script>
import apexchart from 'vue-apexcharts'

export default {
  components: { apexchart },
  data() {
    return {
      start_date:null,
      end_date:null,
      report_overlay: false,
      is_ready: false,
      barChartOptions: {
        chart: {
          stacked: true,
          type: 'bar',
          toolbar: { show: false },
        },
        grid: {
          padding: {
            top: -20,
            bottom: -10,
          },
          yaxis: {
            lines: { show: false },
          },
        },
        xaxis: {
          categories: [
            ' 9:00',
            ' 9:30',
            ' 10:00',
            ' 10:30',
            ' 11:00',
            ' 11:30',
            ' 12:00',
          ],
          labels: {
            style: {
              colors: '#b9b9c3',
              fontSize: '0.86rem',
            },
          },
          axisTicks: {
            show: false,
          },
          axisBorder: {
            show: false,
          },
        },
        legend: {
          show: false,
        },
        dataLabels: {
          enabled: false,
        },
        colors: ['#3ED3BB', '#FD4863', '#39539E', '#0077B5'],
        plotOptions: {
          bar: {
            columnWidth: '17%',
            endingShape: 'rounded',
          },
          distributed: true,
        },
        yaxis: {
          labels: {
            style: {
              colors: '#b9b9c3',
              fontSize: '0.86rem',
            },
          },
        },
      },
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
        series: [85, 16, 50, 50],
        legend: {
          show: true,
          position: 'bottom',
          fontSize: '14px',
          fontFamily: 'Montserrat',
        },
        colors: ['#3ED3BB', '#FD4863', '#39539E', '#0077B5'],
        dataLabels: {
          enabled: true,
          formatter(val) {
            // eslint-disable-next-line radix
            return `${parseInt(val)}%`
          },
        },
        plotOptions: {
          pie: {
            donut: {
              labels: {
                show: true,
                name: {
                  fontSize: '2rem',
                  fontFamily: 'Montserrat',
                },
                value: {
                  fontSize: '1rem',
                  fontFamily: 'Montserrat',
                  formatter(val) {
                    // eslint-disable-next-line radix
                    return `${parseInt(val)}%`
                  },
                },
                total: {
                  show: true,
                  fontSize: '1.5rem',
                  label: 'Operational',
                  formatter() {
                    return '31%'
                  },
                },
              },
            },
          },
        },
        labels: ['Operational', 'Networking', 'Hiring', 'R&D'],
        responsive: [
          {
            breakpoint: 992,
            options: {
              chart: {
                height: 380,
              },
              legend: {
                position: 'bottom',
              },
            },
          },
          {
            breakpoint: 576,
            options: {
              chart: {
                height: 320,
              },
              plotOptions: {
                pie: {
                  donut: {
                    labels: {
                      show: true,
                      name: {
                        fontSize: '1.5rem',
                      },
                      value: {
                        fontSize: '1rem',
                      },
                      total: {
                        fontSize: '1.5rem',
                      },
                    },
                  },
                },
              },
              legend: {
                show: true,
              },
            },
          },
        ],
      },
      areaOptions: {
        chart: {
          toolbar: {
            show: false,
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          show: false,
          curve: 'straight',
        },
        legend: {
          show: true,
          position: 'top',
          horizontalAlign: 'left',
          fontSize: '14px',
          fontFamily: 'Montserrat',
        },
        grid: {
          xaxis: {
            lines: {
              show: true,
            },
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
        },
        yaxis: {
          // opposite: isRtl
        },
        fill: {
          opacity: 1,
          type: 'solid',
        },
        tooltip: {
          shared: false,
        },
        colors: ['#3AA1F8', '#FCBA50', '#FD4863', '#a4f8cd'],
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
    this.getGateInfo(this.$route.query.id)
  },
  watch: {
    end_date(val) {
      this.report_overlay = true
      setTimeout(() => {
        this.report_overlay = false
      }, 500)
    },
  },
  methods: {
    async getGateInfo() {
      try {
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/gates/' + 3
        )
        if (response && response.data && response.data.reports) {
          this.gate_report = response.data.reports
          this.is_ready = true
        }
      } catch (error) {
        console.log(error)
      }
    },
    formatDate(date, type) {
      let return_date
      switch (type) {
        case 'text':
          return_date = this.moment(date).format('Do MMMM YYYY')
          break
        case 'report_time':
          return_date = this.moment(date).format('DD-MM-YYYY')
          break
        case 'text_hours':
          return_date = this.moment(date).format('Do MMMM YYYY H:mm')
          break
        case 'std_hours':
          return_date = this.moment(date).format('DD/MM/YYYY H:mm')
          break
        case 'database_time':
          return_date = this.moment(date).format('YYYY-MM-DD H:mm:ss')
          break
        case 'database':
          return_date = this.moment(date).format('YYYY-MM-DD')
          break
        default:
          return_date = this.moment(date).format('DD/MM/YYYY')
          break
      }
      return return_date
    },
  },
}
</script>

<style></style>
