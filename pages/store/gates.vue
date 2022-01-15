<template>
  <b-overlay :show="gates_overlay" rounded="sm">
    <div class="basic-bg">
      <div class="p-5 text-center" v-if="gates && gates.length > 0">
        <img src="@/assets/images/logo.png" width="180" alt="" />
        <h5 class="d-block mt-2 mb-3">Information of the doors of the store</h5>
        <div class="mt-4">
          <b-row>
            <b-col md="6" offset-md="3">
              <div class="text-right">
                <b-button
                  variant="primary"
                  class="mb-3 p-1"
                  v-b-modal.modal-prevent-closing
                >
                  New Gate
                </b-button>
              </div>
              <b-list-group>
                <b-list-group-item class="bg-light align-items-center p-2">
                  <b-row>
                    <b-col lg="4" class="text-left">
                      <small><b>Company</b></small>
                    </b-col>
                    <b-col lg="4" class="text-left">
                      <small><b>Gate Name</b></small>
                    </b-col>
                    <b-col lg="2" class="text-left">
                      <small><b>Status</b></small>
                    </b-col>
                    <b-col lg="2" class="text-right"> </b-col>
                  </b-row>
                </b-list-group-item>
                <template v-for="gate in gates">
                  <b-list-group-item
                    :key="gate.id"
                    class="shadow align-items-center p-2"
                  >
                    <b-row>
                      <b-col lg="4" class="text-left">
                        <small>{{ gate.company }}</small>
                      </b-col>
                      <b-col lg="4" class="text-left"
                        ><small>Name:</small>
                        <small
                          ><b>{{ gate.name }}</b></small
                        ></b-col
                      >
                      <b-col lg="1" class="text-right">
                        <b-form-checkbox
                          v-model="gate.status"
                          switch
                          value="true"
                          @change="updateGate(gate)"
                          unchecked-value="false"
                        >
                        </b-form-checkbox>
                      </b-col>
                      <b-col lg="3" class="text-right">
                        <b-button
                          style="padding: 0px 10px 3px 10px"
                          class="text-white"
                          variant="info"
                          @click="detailGate(gate)"
                        >
                          <b-icon icon="eye" font-scale="0.9"></b-icon>
                        </b-button>
                        <b-button
                          variant="danger"
                          style="padding: 0px 10px 3px 10px"
                          @click="deleteGate(gate.id)"
                        >
                          <b-icon icon="trash-fill" font-scale="0.9"></b-icon>
                        </b-button>
                      </b-col>
                    </b-row>
                  </b-list-group-item>
                </template>
              </b-list-group>
              <b-modal
                id="modal-prevent-closing"
                ref="gate_modal"
                title="Add New Gate"
                @ok="setGate"
              >
                <form ref="form">
                  <b-form-group label="Gate Name">
                    <b-form-input
                      id="name-input"
                      v-model="gate_name"
                    ></b-form-input>
                  </b-form-group>
                  <b-form-group label="Status">
                    <b-form-checkbox
                      v-model="gate_status"
                      switch
                      value="true"
                      unchecked-value="false"
                    >
                    </b-form-checkbox>
                  </b-form-group>
                </form>
              </b-modal>
            </b-col>
          </b-row>
        </div>
      </div>
    </div>
  </b-overlay>
</template>

<script>
export default {
  data() {
    return {
      gates: [],
      gate_name: null,
      gate_status: null,
      gates_overlay: false,
    }
  },
  async created() {
    this.getShopDoors()
  },
  methods: {
    detailGate(gate) {
      let id = gate.id
      this.$router.push({
        name: `store-gatedetail`,
        query: { id },
      })
    },
    async setGate() {
      let payload = {
        company: localStorage.getItem('selected_store'),
        name: this.gate_name,
        status: this.gate_status,
      }
      try {
        const response = await this.$axios.post(
          'https://61e0849f63f8fc0017618805.mockapi.io/gates',
          payload
        )
        if (response) {
          console.log(response)
          alert('shop door added successfully!')
          this.getShopDoors()
        }
      } catch (error) {
        console.log(error)
      }
    },
    async updateGate(gate) {
      if (gate.id) {
        let payload = gate
        try {
          this.gates_overlay = true
          const response = await this.$axios.put(
            'https://61e0849f63f8fc0017618805.mockapi.io/gates/' + gate.id,
            payload
          )
          if (response) {
            console.log(response, 'gateupdate')
            alert('gate has been successfully updated')
          }
        } catch (error) {
          console.log(error)
          alert('Error')
        } finally {
          this.gates_overlay = false
        }
      }
    },
    async deleteGate(gate_id) {
      if (gate_id) {
        try {
          this.gates_overlay = true
          const response = await this.$axios.delete(
            'https://61e0849f63f8fc0017618805.mockapi.io/gates/' + gate_id
          )
          if (response) {
            alert('Gate successfully removed')
            this.getShopDoors()
          }
        } catch (error) {
          console.log(error)
          alert('Error')
        } finally {
          this.gates_overlay = false
        }
      }
    },
    async getShopDoors() {
      try {
        this.gates_overlay = true
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/gates'
        )
        if (response && response.data) {
          this.gates = response.data
        }
      } catch (error) {
        console.log(error)
      } finally {
        this.gates_overlay = false
      }
    },
    addNewGate() {},
  },
}
</script>

<style></style>
