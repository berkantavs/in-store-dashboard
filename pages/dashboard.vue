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
                    class="bg-light"
                    placeholder="Start Date"
                    v-model="start_date"
                  ></b-form-datepicker>
                </b-col>
                <b-col>
                  <b-form-datepicker
                    class="bg-light"
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
                    type="line"
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
      start_date: null,
      end_date: null,
      report_overlay: false,
      is_ready: false,
      barChartOptions: {
        chart: {
          height: 350,
          type: 'line',
          zoom: {
            enabled: false,
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: 'straight',
        },
        title: {
          text: 'Product Trends by Month',
          align: 'left',
        },
        grid: {
          row: {
            colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
            opacity: 0.5,
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
          data: [],
        },
        {
          name: 'erkek_reyon',
          data: [],
        },
        {
          name: 'kadin_reyon',
          data: [],
        },
        {
          name: 'bebek_reyon',
          data: [],
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
          this.formatReport(this.gate_report)
          this.is_ready = true
        }
      } catch (error) {
        console.log(error)
      }
    },
    formatReport(data) {
      this.series[0].data = []
      this.series[1].data = []
      this.series[2].data = []
      this.series[3].data = []

      data.forEach((value) => {
        let erkek_reyon = null
        let kadin_reyon = null
        let yaz_reyon = null
        let bebek_reyon = null
        for (let [key_in, value_in] of Object.entries(value)) {
          switch (key_in) {
            case 'erkek_reyon':
              erkek_reyon = parseFloat(Number(value_in)).toFixed(2)
              break
            case 'kadin_reyon':
              kadin_reyon = parseFloat(Number(value_in)).toFixed(2)
              break
            case 'bebek_reyon':
              bebek_reyon = parseFloat(Number(value_in)).toFixed(2)
              break
            case 'yaz_reyon':
              yaz_reyon = parseFloat(Number(value_in)).toFixed(2)
              break
            default:
              break
          }
        }
        this.series[0].data.push(yaz_reyon)
        this.series[1].data.push(erkek_reyon)
        this.series[2].data.push(kadin_reyon)
        this.series[3].data.push(bebek_reyon)
      })
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
