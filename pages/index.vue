<template>
  <body class="bg-login">
    <template v-if="selected_shopping_center">
      <div class="wrapper">
        <div
          class="section-authentication-signin d-flex align-items-center justify-content-center my-5 my-lg-0"
        >
          <div class="container-fluid">
            <div class="row row-cols-1 row-cols-lg-2 row-cols-xl-3">
              <div class="col mx-auto">
                <div class="mb-4 text-center">
                  <img src="@/assets/images/logo.png" width="180" alt="" />
                </div>
                <div class="card">
                  <div class="card-body">
                    <div class="border p-4 rounded">
                      <div class="form-body">
                        <form class="row g-3">
                          <div class="col-12">
                            <label for="inputEmailAddress" class="form-label"
                              >Email</label
                            >
                            <input
                              type="email"
                              class="form-control"
                              v-model="email"
                              placeholder="Email Address"
                            />
                          </div>
                          <div class="col-12">
                            <label for="inputChoosePassword" class="form-label"
                              >Password</label
                            >
                            <div class="input-group" id="show_hide_password">
                              <input
                                type="password"
                                class="form-control border-end-0"
                                v-model="password"
                                placeholder="Enter Password"
                              />
                              <a
                                href="javascript:;"
                                class="input-group-text bg-transparent"
                                ><i class="bx bx-hide"></i
                              ></a>
                            </div>
                          </div>

                          <div class="col-12">
                            <div class="d-grid">
                              <b-button variant="primary" @click="login()">
                                <i class="bx bxs-lock-open"></i>Sign in
                              </b-button>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!--end row-->
          </div>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="choose-shopping-center text-center m-5">
        <img src="@/assets/images/logo.png" width="180" alt="" />
        <h5 class="d-block mt-2 mb-3">Choose Store</h5>
        <div class="shopping-centers mt-2">
          <b-row>
            <b-col md="6" offset-md="3">
              <b-row>
                <b-col lg="6">
                  <b-card
                    no-body
                    @click="selectStore('koctas')"
                    class="rounded border cursor-pointer text-center shopping-center-card"
                  >
                    <div class="text-center">
                      <img
                        src="@/assets/images/koctas.png"
                        width="180"
                        alt=""
                      />
                    </div>
                  </b-card>
                </b-col>
                <b-col lg="6">
                  <b-card
                    no-body
                    @click="selectStore('civil')"
                    class="rounded border cursor-pointer text-center shopping-center-card"
                  >
                    <div class="text-center">
                      <img src="@/assets/images/civil.png" width="180" alt="" />
                    </div> </b-card
                ></b-col>
                <b-col lg="6">
                  <b-card
                    no-body
                    @click="selectStore('ebebek')"
                    class="rounded border cursor-pointer text-center shopping-center-card"
                  >
                    <div class="text-center">
                      <img
                        src="@/assets/images/ebebek.png"
                        width="180"
                        alt=""
                      />
                    </div>
                  </b-card>
                </b-col>
                <b-col lg="6">
                  <b-card
                    no-body
                    @click="selectStore('evidea')"
                    class="rounded border text-center cursor-pointer shopping-center-card"
                  >
                    <div class="text-center">
                      <img
                        src="@/assets/images/evidea.png"
                        width="180"
                        alt=""
                      /></div></b-card
                ></b-col>
              </b-row>
            </b-col>
          </b-row>
        </div>
      </div>
    </template>
  </body>
</template>

<script>
export default {
  data() {
    return { email: null, password: null, selected_shopping_center: null }
  },
  layout: 'basic',
  methods: {
    authCheck(email, password) {
      if (email == 'info@imona.com' && password == 'imona123') {
        return true
      } else {
        return false
      }
    },
    login() {
      if (this.authCheck(this.email, this.password)) {
        this.$router.push('/storedetails')
      }
    },
    selectStore(s_center) {
      if (s_center) {
        localStorage.setItem('selected_store', s_center)
        this.selected_shopping_center = s_center
      }
    },
  },
}
</script>

<style></style>
