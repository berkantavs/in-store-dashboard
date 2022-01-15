<template>
  <div class="basic-bg">
    <div class="text-center p-5">
      <img src="@/assets/images/logo.png" width="180" alt="" />
      <h5 class="d-block mt-2 mb-3">Choose Store Analytics</h5>
      <div class="mt-4">
        <b-row>
          <b-col md="6" offset-md="3">
            <b-row>
              <b-col lg="6">
                <b-card
                  class="text-center cursor-pointer"
                  @click="goToShopDoors()"
                >
                  <template>
                    <b-img width="83px" src="@/assets/images/gate.png"> </b-img>
                  </template>
                  <div class="truncate mb-50">
                    <h6 class="mb-0 mt-1 text-primary font-weight-bolder">
                      Gates
                    </h6>
                  </div>
                </b-card>
              </b-col>
              <b-col lg="6">
                <b-card class="text-center cursor-pointer">
                  <template>
                    <b-img width="100px" src="@/assets/images/cashier.png">
                    </b-img>
                  </template>
                  <div class="truncate mb-50">
                    <h6 class="mb-0 mt-1 text-primary font-weight-bolder">
                      Tills
                    </h6>
                  </div>
                </b-card>
              </b-col>
              <b-col lg="12">
                <b-card class="text-center cursor-pointer">
                  <template>
                    <b-img width="100px" src="@/assets/images/rayon.png">
                    </b-img>
                  </template>
                  <div class="truncate mb-50">
                    <h6 class="mb-0 mt-1 text-primary font-weight-bolder">
                      Rayons
                    </h6>
                  </div>
                </b-card>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'default',
  methods: {
    goToShopDoors() {
      this.$router.push('/store/gates')
    },
  },
}
</script>

<style></style>
