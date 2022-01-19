<template>
  <div class="mt-3">
    <div class="mt-4">
      <b-card no-body class="shadow rounded">
        <b-card-header>
          <b-row>
            <b-col lg="5" class="text-left">
              <h5 class="mt-3">Ebebek Stores</h5>
            </b-col>
            <b-col lg="7" class="text-right">
              <b-row>
                <b-col lg="8">
                  <b-form-input
                    v-model="search_item"
                    placeholder="search by store name"
                  ></b-form-input>
                </b-col>
                <b-col lg="4">
                  <b-button
                    variant="danger"
                    @click="search_item = ''"
                    v-if="search_item && search_item.length > 0"
                  >
                    <b-icon icon="x"></b-icon>
                  </b-button>
                  <b-button variant="primary" v-b-modal.modal-prevent-closing>
                    New Store
                  </b-button>
                </b-col>
              </b-row>
            </b-col>
          </b-row>
        </b-card-header>
        <b-card-body>
          <table
            class="table table-hover table-border border"
            v-if="stores && stores.length > 0"
          >
            <thead>
              <tr>
                <th class="text-center" scope="col">#</th>
                <th class="text-center" scope="col">Company</th>
                <th class="text-center" scope="col">Store Name</th>
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
                  <td class="text-center">Ebebek</td>
                  <td class="text-center">{{ store.name }}</td>
                  <td class="text-center">{{ store.country }}</td>
                  <td class="text-center">{{ store.city }}</td>
                  <td class="text-center">
                    {{ store.district ? store.district : '-' }}
                  </td>
                  <td class="text-center">
                    <b-form-checkbox
                      v-model="store.status"
                      switch
                      size="lg"
                      @change="updateStore(store)"
                      value="true"
                      unchecked-value="false"
                    >
                    </b-form-checkbox>
                  </td>
                </tr>
              </template>
            </tbody>
          </table>
          <div v-else class="text-center mb-2 mt-2">store not found</div>
          <b-modal
            id="modal-prevent-closing"
            ref="gate_modal"
            title="Add New Ebebek Store"
            @ok="setStore"
          >
            <form ref="form">
              <b-form-group label="Store Name">
                <b-form-input v-model="store_name"></b-form-input>
              </b-form-group>
              <b-form-group label="Country">
                <b-form-input v-model="country"></b-form-input>
              </b-form-group>
              <b-form-group label="City">
                <b-form-input v-model="city"></b-form-input>
              </b-form-group>
              <b-form-group label="District">
                <b-form-input v-model="district"></b-form-input>
              </b-form-group>
              <b-form-group label="Status">
                <b-form-checkbox
                  v-model="store_status"
                  switch
                  value="true"
                  unchecked-value="false"
                >
                </b-form-checkbox>
              </b-form-group>
            </form>
          </b-modal>
          <div class="text-left mt-1">
            <b-button
              variant="primary"
              @click="detailStore"
              v-if="selected_stores && selected_stores.length > 0"
            >
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
      store_name: null,
      country: null,
      city: null,
      district: null,
      search_item: '',
      store_status: null,
    }
  },
  async created() {
    this.getStores()
  },
  watch: {
    search_item(val) {
      if (val) {
        this.searchStoreByName()
      } else {
        this.getStores()
      }
    },
  },
  methods: {
    async searchStoreByName() {
      try {
        const foundedStores = this.stores.filter((store) =>
          store.name.toLowerCase().includes(this.search_item.toLowerCase())
        )
        this.stores = JSON.parse(JSON.stringify(foundedStores))
      } catch (error) {
        console.log(error)
      }
    },
    async updateStore(store) {
      if (store.id) {
        let payload = store
        try {
          const response = await this.$axios.put(
            'https://61e0849f63f8fc0017618805.mockapi.io/stores/' + store.id,
            payload
          )
          if (response) {
            console.log(response)
            alert('store has been successfully updated')
          }
        } catch (error) {
          console.log(error)
          alert('Error')
        }
      }
    },
    async setStore() {
      let payload = {
        name: this.store_name,
        country: this.country,
        city: this.city,
        district: this.district,
        status: this.store_status,
      }
      try {
        const response = await this.$axios.post(
          'https://61e0849f63f8fc0017618805.mockapi.io/stores',
          payload
        )
        if (response) {
          alert('store added successfully')
          this.getStores()
        }
      } catch (error) {
        console.log(error)
        alert('Error')
      }
    },
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
      this.$router.push('/store-details')
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
