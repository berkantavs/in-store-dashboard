<template>
  <div>
    <div class="text-left">
      <template>
        <div>
          <div class="mt-4">
            <b-card class="shadow rounded" no-body>
              <b-card-header>
                <h5 class="mt-2 mb-2">Store Details</h5>
              </b-card-header>
              <b-card-body>
                <template v-if="store_sections && store_sections.length > 0">
                  <table class="table table-hover table-border border">
                    <thead>
                      <tr>
                        <th class="text-center" scope="col">#</th>
                        <th class="text-center" scope="col">Name</th>
                        <th class="text-center" scope="col">Section</th>
                        <th class="text-center" scope="col">Type</th>
                        <th class="text-center" scope="col">Status</th>
                      </tr>
                    </thead>
                    <tbody>
                      <template v-for="section in store_sections">
                        <tr :key="section.id">
                          <td class="text-center" scope="row">
                            <b-form-checkbox
                              @change="selectedSections($event, section)"
                            >
                            </b-form-checkbox>
                          </td>
                          <td class="text-center">{{ section.name }}</td>
                          <td class="text-center">{{ section.section }}</td>
                          <td class="text-center">{{ section.type }}</td>
                          <td class="text-center">
                            <b-form-checkbox
                              v-model="section.status"
                              switch
                              size="lg"
                              value="true"
                              unchecked-value="false"
                            >
                            </b-form-checkbox>
                          </td>
                        </tr>
                      </template>
                    </tbody>
                  </table>
                </template>
                <div class="text-left mt-1">
                  <b-button variant="primary" @click="showDetail">
                    list selected sections
                  </b-button>
                  <b-button variant="info" class="text-white" @click="showDetail">
                    list all sections
                  </b-button>
                </div>
              </b-card-body>
            </b-card>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'default',
  data() {
    return {
      store_sections: [],
      stores: [],
      selected_sections: [],
    }
  },
  async created() {
    this.getSections()
  },
  methods: {
    async getSections() {
      try {
        let sections = []
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/stores'
        )

        if (response && response.data) {
          response.data.forEach((store) => {
            if (store.sections && store.sections.length > 0) {
              sections.push(store.sections[0])
            }
          })
        }
        this.store_sections = sections
      } catch (error) {
        console.log(error)
        alert('error')
      }
    },
    showDetail() {
      this.$router.push('/dashboard')
    },
    removeDuplicates(arr) {
      const ids = arr.map((o) => o.id)
      const filtered = arr.filter(
        ({ id }, index) => !ids.includes(id, index + 1)
      )
      this.selected_sections = filtered
    },
    selectedSections(event, section) {
      if (section) {
        if (event == true) {
          if (this.selected_sections && this.selected_sections.length > 0) {
            this.selected_sections.forEach((s_section) => {
              if (section.id != s_section.id) {
                this.selected_sections.push(section)
                this.removeDuplicates(this.selected_sections)
              }
            })
          } else {
            this.selected_sections.push(section)
          }
        } else {
          this.selected_sections.forEach((s_section, index) => {
            if (section.id == s_section.id) {
              this.selected_sections.splice(index, 1)
            }
          })
        }

        localStorage.setItem('sections', JSON.stringify(this.selected_sections))
        console.log(this.selected_sections, 'sections')
      }
    },
    goToShopDoors() {
      this.$router.push('/store/gates')
    },
    goToShopCases() {
      this.$router.push('/store/cases')
    },
  },
}
</script>

<style></style>
