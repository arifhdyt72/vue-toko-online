<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>List of <strong>Foods</strong></h2>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Search your like a food"
              aria-label="Search your like a food"
              aria-describedby="basic-addon1"
              @keyup="seachFoods"
            />
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1">
                <b-icon-search></b-icon-search>
              </span>
            </div>
          </div>
        </div>
      </div>

      <div class="row mb-4">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          v-bind:key="product.id"
        >
          <CardProduct v-bind:product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    async seachFoods() {
      let response = {};
      if(this.search == ""){
        response = await axios.get("products")
      .catch((error) => console.log("Gagal: ", error));
      }else{
        response = await axios.get("search-products/"+this.search)
          .catch((error) => console.log("Gagal: ", error));
      }
      
      if(response.data.meta.code == 200){
        const result = response.data.data
        if(result != null){
          this.setProduct(result)
        }
      }
    },
  },
  async mounted() {
    const response = await axios.get("products")
      .catch((error) => console.log("Gagal: ", error));
    
    if(response.data.meta.code == 200){
      const result = response.data.data
      this.setProduct(result)
    }
  },
};
</script>