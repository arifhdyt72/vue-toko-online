<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link class="text-dark" to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link class="text-dark" to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
            <img :src="'../assets/images/'+product.gambar" :alt="product.nama" class="img-fluid shadow" />
        </div>
        <div class="col-md-6">
            <h2><strong>{{ product.nama }}</strong></h2>
            <hr />
            <h4>Price: <strong>Rp. {{ product.harga }}</strong></h4>
            <form class="mt-4">
                <div class="form-group">
                    <label for="qty-order">Total Order</label>
                    <input type="number" id="qty-order" class="form-control" />
                </div>
                <div class="form-group">
                    <label for="desc-order">Description</label>
                    <textarea class="form-control" id="desc-order" placeholder="Example: Dont use sambal.."></textarea>
                </div>
                <button type="submit" class="btn btn-success"><b-icon-cart></b-icon-cart> Order</button>
            </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/"+this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log("Gagal: ", error));
  },
};
</script>

<style scoped>
.breadcrumb {
  background-color: transparent;
  padding: 0;
}

.breadcrumb-item.active {
  font-weight: bold;
  color: #000;
}

.img-fluid {
    border-radius: 15px;
}

</style>
