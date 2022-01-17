<template>
  <div class="mt-4">
    <b-overlay :show="case_overlay" rounded="sm">
      <div class="pl-0 pt-4 pr-4" v-if="cases && cases.length > 0">
        <img src="@/assets/images/logo.png" width="180" alt="" />
        <div class="mt-4">
          <b-card no-body class="shadow rounded">
            <b-card-header>
              <b-row>
                <b-col class="text-left">
                  <h5 class="mt-2 mb-2">
                    Information of the case of the store
                  </h5>
                </b-col>
                <b-col class="text-right">
                  <b-button
                    variant="primary"
                    class="mb-2 mt-2 p-1"
                    v-b-modal.modal-prevent-closing
                  >
                    New Case
                  </b-button>
                </b-col>
              </b-row>
            </b-card-header>
            <b-card-body>
              <table class="table table-hover table-border border">
                <thead>
                  <tr>
                    <th scope="col" class="text-center">#</th>
                    <th scope="col" class="text-center">Company</th>
                    <th scope="col" class="text-center">Name</th>
                    <th scope="col" class="text-center">Status</th>
                    <th scope="col" class="text-center">Operations</th>
                  </tr>
                </thead>
                <tbody>
                  <template v-for="(shop_case, index) in cases">
                    <tr :key="index">
                      <td scope="row" class="text-center">
                        {{ shop_case.id }}
                      </td>
                      <td class="text-center">{{ shop_case.company }}</td>
                      <td class="text-center">{{ shop_case.name }}</td>
                      <td class="text-center">
                        <b-form-checkbox
                          v-model="shop_case.status"
                          switch
                          value="true"
                          size="lg"
                          @change="updateCase(shop_case)"
                          unchecked-value="false"
                        >
                        </b-form-checkbox>
                      </td>
                      <td class="text-center">
                        <b-button
                          style="padding: 0px 10px 3px 10px"
                          class="text-white"
                          variant="info"
                          @click="detailCase(shop_case)"
                        >
                          <b-icon icon="eye" font-scale="0.9"></b-icon>
                        </b-button>
                        <b-button
                          variant="danger"
                          style="padding: 0px 10px 3px 10px"
                          @click="deleteCase(shop_case.id)"
                        >
                          <b-icon icon="trash-fill" font-scale="0.9"></b-icon>
                        </b-button>
                      </td>
                    </tr>
                  </template>
                  <b-modal
                    id="modal-prevent-closing"
                    ref="case_modal"
                    title="Add New Case"
                    @ok="setCase"
                  >
                    <form ref="form">
                      <b-form-group label="Case Name">
                        <b-form-input
                          id="name-input"
                          v-model="case_name"
                        ></b-form-input>
                      </b-form-group>
                      <b-form-group label="Status">
                        <b-form-checkbox
                          v-model="case_status"
                          switch
                          value="true"
                          unchecked-value="false"
                        >
                        </b-form-checkbox>
                      </b-form-group>
                    </form>
                  </b-modal>
                </tbody>
              </table>
            </b-card-body>
          </b-card>
        </div>
      </div>
    </b-overlay>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cases: [],
      case_name: null,
      case_status: null,
      case_overlay: false,
    }
  },
  created() {
    this.getCases()
  },
  methods: {
    async getCases() {
      try {
        this.case_overlay = true
        const response = await this.$axios.get(
          'https://61e0849f63f8fc0017618805.mockapi.io/cases'
        )
        if (response && response.data) {
          this.cases = response.data
        }
      } catch (error) {
        console.log(error)
        alert('Error')
      } finally {
        this.case_overlay = false
      }
    },
    async setCase() {
      let payload = {
        company: localStorage.getItem('selected_store'),
        name: this.case_name,
        status: this.case_status,
      }
      try {
        const response = await this.$axios.post(
          'https://61e0849f63f8fc0017618805.mockapi.io/cases',
          payload
        )
        if (response) {
          console.log(response)
          alert('case added successfully!')
          this.getCases()
        }
      } catch (error) {
        console.log(error)
      }
    },
    detailCase(shop_case) {
      let id = shop_case.id
      this.$router.push({
        name: `store-casedetail`,
        query: { id },
      })
    },
    async updateCase(shop_case) {
      if (shop_case.id) {
        let payload = shop_case
        try {
          this.case_overlay = true
          const response = await this.$axios.put(
            'https://61e0849f63f8fc0017618805.mockapi.io/cases/' + shop_case.id,
            payload
          )
          if (response) {
            alert('case has been successfully updated')
          }
        } catch (error) {
          console.log(error)
          alert('Error')
        } finally {
          this.case_overlay = false
        }
      }
    },
    async deleteCase(shop_case_id) {
      if (shop_case_id) {
        try {
          this.case_overlay = true
          const response = await this.$axios.delete(
            'https://61e0849f63f8fc0017618805.mockapi.io/cases/' + shop_case_id
          )
          if (response) {
            alert('case successfully removed')
            this.getShopDoors()
          }
        } catch (error) {
          console.log(error)
          alert('Error')
        } finally {
          this.case_overlay = false
        }
      }
    },
  },
}
</script>

<style></style>
