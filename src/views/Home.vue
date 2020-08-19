<template>
  <div class="home">
    <b-container>
      <img alt="Vue logo" src="../assets/logo.png" />
      <Navbar />
      <b-card>
        <h4>My Name is {{ name }}</h4>
      </b-card>
      <b-card>
        <h5>Computed</h5>
        <p>Original Message : {{ message }}</p>
        <p>Reverse Message : {{ reverseMessage }}</p>
        <input type="text" v-model="greeting" />
        <p>Computed Setter : {{ greeting }}</p>
      </b-card>
      <b-card>
        <h5>Directive</h5>
        <!-- v-if -->
        <div v-if="isFulled === true">{{ name }}</div>
        <div v-else-if="isFulled === 'tampil'">Itzy</div>
        <div v-else>Tri</div>
        <div v-show="isFulled === true">V Show : {{ name }}</div>
        <!-- v-for -->
        <ul>
          <li v-for="(value, index) in product" :key="index">{{ value.name }} - {{ value.price }}</li>
        </ul>
        <!-- v-on -->
        <button @click="boom">Click Me !</button>
        <input type="text" v-on:keyup.enter="search" />
        <br />
        <!-- v-bind -->
        <a
          :href="url_google"
          v-bind:style="url_google === 'http://google.com' ? isTrue : isFalse "
        >Click Link Google</a>
      </b-card>
    </b-container>
  </div>
</template>

<script>
import Navbar from '../components/_base/Navbar'

export default {
  name: 'Home',
  data() {
    return {
      name: 'Arqi Alfaritsi',
      message: 'Hello',
      a: 'Hai',
      b: 'Arqi',
      isFulled: false,
      product: [
        { name: 'kipas', price: 100000 },
        { name: 'mouse', price: 50000 }
      ],
      url_google: 'http://google.com',
      isTrue: {
        color: 'black'
      },
      isFalse: {
        color: 'red'
      }
    }
  },
  components: {
    Navbar
  },
  computed: {
    reverseMessage() {
      return this.message.split('').reverse().join('')
    },
    methods: {
      boom() {
        console.log('Boom !')
        alert('Boom !')
      },
      search() {
        console.log('Search !')
        alert('Search !')
      }
    },
    greeting: {
      get() {
        return this.a + ' ' + this.b
      },
      set(newValue) {
        const value = newValue.split(' ')
        this.a = value[0]
        this.b = value[1]
      }
    }
  }
}
</script>

<style scoped>
.home {
  text-align: center;
}
</style>
