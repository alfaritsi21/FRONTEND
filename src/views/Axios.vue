<template>
  <div class="axios">
    <h1>Axios Page</h1>
    <b-container>
      <b-alert :show="true">{{isMessage}}</b-alert>
      <form v-on:submit.prevent="addProduct">
        <input type="text" v-model="form.product_name" placeholder="Product Name" />
        <br />
        <input type="text" v-model="form.product_harga" placeholder="Product Harga" />
        <br />
        <input type="text" v-model="form.category_id" placeholder="Category Id" />
        <br />
        <input type="text" v-model="form.product_status" placeholder="Product Status" />
        <br />
        <button type="submit" v-show="!isUpdate">Simpan</button>
        <button type="submit" v-show="isUpdate" @click="patchProduct()">Update</button>
      </form>
    </b-container>
    <hr />
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="4" v-for="(item, index) in products" :key="index">
          <b-card
            v-bind:title="item.product_name"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
          >
            <b-card-text>{{item.product_harga}}</b-card-text>

            <b-button variant="primary" @click="addToCart(item)">Add To Cart</b-button>
            <b-button variant="success" @click="setProduct(item)">Update</b-button>
            <b-button variant="danger" @click="deleteProduct(item)">Delete</b-button>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
    <Card nama="Kopi" harga="2000" @increment="incrementCount" />
    <Card nama="Susu" harga="3000" />
    <p>{{ count }}</p>
  </div>
</template>

<script>
import axios from 'axios'
import Card from '../components/_base/Card'

export default {
  name: 'Axios',
  components: {
    Card
  },
  data() {
    return {
      count: 0,
      cart: [],
      page: 1,
      limit: 4,
      sort: '',
      products: [],
      form: {
        category_id: '',
        product_name: '',
        product_harga: '',
        product_status: ''
      },
      alert: false,
      isMessage: '',
      isUpdate: false
    }
  },
  created() {
    this.get_product()
  },
  methods: {
    incrementCount(data) {
      this.count += data
    },
    addToCart(data) {
      const setCart = {
        product_id: data.product_id,
        quantity: 1
      }
      this.cart = [...this.cart, setCart]
      console.log(this.cart)
    },
    get_product() {
      axios
        .get(
          `http://127.0.0.1:3001/product?page=${this.page}&limit=${this.limit}`
        )
        .then((response) => {
          this.products = response.data.data
          this.alert = true
          this.isMessage = response.data.msg
        })
        .catch((error) => {
          console.log(error)
        })
    },
    addProduct() {
      console.log(this.form)
    },
    setProduct(data) {
      this.form = {
        product_name: data.product_name,
        category_id: data.category_id,
        product_harga: data.product_harga,
        product_status: data.product_status
      }
      this.isUpdate = true
      this.product_id = data.product_id
    },
    patchProduct() {
      console.log(this.product_id)
      console.log(this.form)
      this.isUpdate = false
    },
    deleteProduct(data) {
      console.log(data.product_id)
    }
  }
}
</script>
