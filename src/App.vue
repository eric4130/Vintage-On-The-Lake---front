<template>
  <div id="wrapper">
    <nav class="navbar is-black is-fixed-top">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item" @click="showMobileMenu = false"><img src="./assets/logo.png" class="logo"><strong>Vintage On The Lake</strong></router-link>
        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu }">
        <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" class="input" placeholder="What are you looking for?" name="query">
                </div>

                <div class="control">
                  <button class="button is-success">
                      <span class="icon">
                      <i class="fas fa-search"></i>
                      </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <div class="navbar-end">
          <router-link to="/50s" class="navbar-item"  @click="showMobileMenu = false">50s Era</router-link>
          <router-link to="/60s" class="navbar-item" @click="showMobileMenu = false">60s Era</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <template v-if="$store.state.isAuthenticated">
                <router-link to="/my-account" class="button is-info" @click="showMobileMenu = false">My Account</router-link>
              </template>

              <template v-else>
                <router-link to="/log-in" class="button is-light" @click="showMobileMenu = false">Log In</router-link>
              </template>

              <router-link to="/cart" class="button is-success" @click="showMobileMenu = false">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section" style="padding-bottom:18px;">
      <router-view/>
    </section>

    <footer class="footer footerstuff">
      <div class="content has-text-centered">
      <p class="has-text-centered" style="margin-bottom:0px;">Copyright (c) 2021</p>
      <router-link to="/about" @click="showMobileMenu = false" style="text-decoration:underline; color:#FF0000;">About VOTL</router-link>
     <br>
      <img src="./assets/logo.png" class="footerlogo">
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },
  beforeCreate() {
    this.$store.commit('initializeStore')

    const token = this.$store.state.token

    if (token) {
        axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
        axios.defaults.headers.common['Authorization'] = ""
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
      cartTotalLength() {
          let totalLength = 0

          for (let i = 0; i < this.cart.items.length; i++) {
              totalLength += this.cart.items[i].quantity
          }

          return totalLength
      }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;

  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 80px;
  }
}

.logo {
  border-radius:50%;
  margin-right:5px;
}

.footerlogo {
  border-radius:50%;
  width:50px;
  height:50px;
  margin-top:5px;
}

.footerstuff {
  background-color: #C2D107;
  padding: 0px 0px 0px;
}

body {
background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' version='1.1' xmlns:xlink='http://www.w3.org/1999/xlink' xmlns:svgjs='http://svgjs.com/svgjs' width='1440' height='560' preserveAspectRatio='none' viewBox='0 0 1440 560'%3e%3cg mask='url(%26quot%3b%23SvgjsMask1000%26quot%3b)' fill='none'%3e%3crect width='1440' height='560' x='0' y='0' fill='rgba(255%2c 81%2c 0%2c 1)'%3e%3c/rect%3e%3cpath d='M 0%2c221 C 57.6%2c192 172.8%2c77.6 288%2c76 C 403.2%2c74.4 460.8%2c207.6 576%2c213 C 691.2%2c218.4 748.8%2c111.2 864%2c103 C 979.2%2c94.8 1036.8%2c176.6 1152%2c172 C 1267.2%2c167.4 1382.4%2c98.4 1440%2c80L1440 560L0 560z' fill='rgba(63%2c 164%2c 24%2c 1)'%3e%3c/path%3e%3cpath d='M 0%2c523 C 72%2c494.6 216%2c400.8 360%2c381 C 504%2c361.2 576%2c425.2 720%2c424 C 864%2c422.8 936%2c355.2 1080%2c375 C 1224%2c394.8 1368%2c493.4 1440%2c523L1440 560L0 560z' fill='rgba(194%2c 209%2c 7%2c 1)'%3e%3c/path%3e%3c/g%3e%3cdefs%3e%3cmask id='SvgjsMask1000'%3e%3crect width='1440' height='560' fill='white'%3e%3c/rect%3e%3c/mask%3e%3c/defs%3e%3c/svg%3e");
background-position: center bottom;
background-size: cover;
}


</style>
