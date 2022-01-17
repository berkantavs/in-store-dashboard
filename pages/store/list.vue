<template>
  <div class="mt-3" v-if="stores && stores.length > 0">
    <div class="mt-4">
      <b-card no-body class="shadow rounded">
        <b-card-header>
          <h5 class="mt-2 mb-2">Stores</h5>
        </b-card-header>
        <b-card-body>
          <table class="table table-hover table-border border">
            <thead>
              <tr>
                <th class="text-center" scope="col">#</th>
                <th class="text-center" scope="col">Store</th>
                <th class="text-center" scope="col">Name</th>
                <th class="text-center" scope="col">Country</th>
                <th class="text-center" scope="col">City</th>
                <th class="text-center" scope="col">District</th>
                <th class="text-center" scope="col">Status</th>
              </tr>
            </thead>
            <tbody>
              <template v-for="store in stores">
                <tr :key="store.id">
                  <td class="text-center" scope="row">
                    <b-form-checkbox @change="selectedStores($event, store)">
                    </b-form-checkbox>
                  </td>
                  <td class="text-center">
                    {{ store.sections[0].store_name }}
                  </td>
                  <td class="text-center">{{ store.name }}</td>
                  <td class="text-center">{{ store.country }}</td>
                  <td class="text-center">{{ store.city }}</td>
                  <td class="text-center">{{ store.district }}</td>
                  <td class="text-center">
                    <b-form-checkbox
                      v-model="store.status"
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
          <div class="text-left mt-1">
            <b-button variant="primary" @click="detailStore">
              list selected stores
            </b-button>
            <b-button variant="info" class="text-white" @click="detailStore">
              list all stores
            </b-button>
          </div>
        </b-card-body>
      </b-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      stores: [],
      store_overlay: false,
      selected_stores: [],
    }
  },
  async created() {
    this.getStores()
  },
  methods: {
    removeDuplicates(arr) {
      const ids = arr.map((o) => o.id)
      const filtered = arr.filter(
        ({ id }, index) => !ids.includes(id, index + 1)
      )
      this.selected_stores = filtered
    },
    selectedStores(event, store) {
      if (store) {
        if (event == true) {
          if (this.selected_stores && this.selected_stores.length > 0) {
            this.selected_stores.forEach((s_store) => {
              if (store.id != s_store.id) {
                this.selected_stores.push(store)
                this.removeDuplicates(this.selected_stores)
              }
            })
          } else {
            this.selected_stores.push(store)
          }
        } else {
          this.selected_stores.forEach((s_store, index) => {
            if (store.id == s_store.id) {
              this.selected_stores.splice(index, 1)
            }
          })
        }

        localStorage.setItem('stores', JSON.stringify(this.selected_stores))
        console.log(this.selected_stores, 'stores')
      }
    },
    detailStore() {
      this.$router.push('/storedetails')
    },
    async getStores() {
      try {
        this.store_overlay = true
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/stores'
        )
        if (response && response.data) {
          this.stores = response.data
        }
      } catch (error) {
        console.log(error)
        alert('Error')
      } finally {
        this.store_overlay = false
      }
    },
  },
}
</script>

<style></style>
