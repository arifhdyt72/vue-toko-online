<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />
      
      <div class="row mt-4">
        <div class="col">
          <h2>Best <strong>Foods</strong></h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right"><b-icon-eye></b-icon-eye> See All</router-link>
        </div>
      </div>

      <div class="row mb-3">
        <div class="col-md-4 mt-4" v-for="product in products" v-bind:key="product.id">
            <CardProduct v-bind:product="product" />
        </div>
      </div>

    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from '@/components/Navbar.vue'
import Hero from '@/components/Hero.vue'
import CardProduct from '@/components/CardProduct.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Navbar,
    Hero,
    CardProduct
  },
  data() {
    return {
      products: []
    }
  },
  methods: {
    setProduct(data) {
      this.products = data
    }
  },
  async mounted() {
    const response = await axios.get('best-product')
      .catch( (error) => console.log("Gagal: ", error));

    if(response.data.meta.code == 200){
      const result = response.data.data;
      this.setProduct(result);
    }
  }
}
</script>
