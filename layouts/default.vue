<template>
  <div>
    <b-row>
      <b-col cols="2">
        <div class="sidebar-wrapper" data-simplebar="true">
          <div class="sidebar-header">
            <div>
              <h4 class="logo-text">imona</h4>
            </div>
            <div class="toggle-icon ms-auto">
              <b-button
                variant="outline-primary"
                style="
                  padding-top: 0px;
                  padding-right: 0px;
                  padding-left: 5px;
                  padding-bottom: 3px;
                "
                @click="logout()"
              >
                <i class="bx bx-arrow-to-left" style="vertical-align: middle">
                </i>
              </b-button>
            </div>
          </div>

          <ul class="metismenu" id="menu">
            <li>
              <a href="javascript:;" class="has-arrow">
                <div class="parent-icon"><i class="bx bx-home-circle"></i></div>
                <div class="menu-title">Dashboard</div>
              </a>
              <ul>
                <li>
                  <a href="/">
                    <b-icon
                      icon="circle"
                      font-scale="0.9"
                      class="mr-2"
                    ></b-icon>
                    Companies</a
                  >
                </li>
                <li>
                  <a href="/store/list">
                    <b-icon
                      icon="circle"
                      font-scale="0.9"
                      class="mr-2"
                    ></b-icon>
                    Stores</a
                  >
                </li>
                <li>
                  <a href="/store/gates">
                    <b-icon
                      icon="circle"
                      font-scale="0.9"
                      class="mr-2"
                    ></b-icon>
                    Gates</a
                  >
                </li>
                <li>
                  <a href="/store/tills">
                    <b-icon
                      icon="circle"
                      font-scale="0.9"
                      class="mr-2"
                    ></b-icon>
                    Tills</a
                  >
                </li>
                <li>
                  <a href="/store/rayons">
                    <b-icon
                      icon="circle"
                      font-scale="0.9"
                      class="mr-2"
                    ></b-icon>
                    Rayons</a
                  >
                </li>
              </ul>
            </li>
          </ul>
        </div>
      </b-col>
      <b-col lg="10">
        <div class="pt-5 pl-2">
          <img src="@/assets/images/logo.png" width="180" alt="" />
          <div class="mt-3">
            <b-breadcrumb class="p-2">
              <b-breadcrumb-item href="/">
                <b-icon
                  icon="house-fill"
                  scale="1.25"
                  shift-v="1.25"
                  aria-hidden="true"
                ></b-icon>
                Home<small class="mr-1"> ></small>
              </b-breadcrumb-item>
              <nuxt-link
                to="/store/list"
                v-if="
                  $route.name == 'store-list' || $route.name == 'storedetails' || $route.name == 'dashboard'
                "
              >
                <b-breadcrumb-item :active="$route.name == 'store-list'">
                  Stores <small class="mr-1"> ></small>
                </b-breadcrumb-item></nuxt-link
              >
              <nuxt-link
                to="/storedetails"
                v-if="
                  $route.name == 'storedetails' || $route.name == 'dashboard'
                "
                ><b-breadcrumb-item :active="$route.name == 'storedetails'"
                  >Sections <small class="mr-1"> ></small>
                </b-breadcrumb-item>
              </nuxt-link>
              <nuxt-link to="/dashboard" v-if="$route.name == 'dashboard'">
                <b-breadcrumb-item :active="$route.name == 'dashboard'">
                  Section Reports</b-breadcrumb-item
                ></nuxt-link
              >
            </b-breadcrumb>
          </div>
        </div>
        <Nuxt />
      </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      store: null,
    }
  },
  async mounted() {
    this.store = localStorage.getItem('selected_store')
  },
  methods: {
    logout() {
      localStorage.removeItem('selected_store')
      this.$router.push('/')
    },
  },
}
</script>

<style>
a {
  text-decoration: none !important;
}
</style>
