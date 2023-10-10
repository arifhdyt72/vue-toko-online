<template>
  <div class="cart">
    <Navbar :updateCarts="carts" />
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
              <li class="breadcrumb-item active" aria-current="page">Cart</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>My <strong>Order</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <th>#</th>
                <th>Picture</th>
                <th>Food Name</th>
                <th>Desc</th>
                <th>Qty</th>
                <th>Price</th>
                <th>Total Price</th>
                <th>Action</th>
              </thead>
              <tbody>
                <tr v-for="(cart, index) in carts" :key="cart.product.id">
                  <td>{{ index + 1 }}</td>
                  <td>
                    <img
                      :src="'http://localhost:8080/' + cart.product.image"
                      :alt="cart.product.name"
                      class="img-fluid shadow"
                      width="200"
                    />
                  </td>
                  <td>
                    <strong>{{ cart.product.name }}</strong>
                  </td>
                  <td>{{ cart.desc ? cart.desc : "-" }}</td>
                  <td>{{ cart.qty }}</td>
                  <td align="right">
                    <strong>Rp.{{ cart.product.price }}</strong>
                  </td>
                  <td align="right">
                    <strong>Rp.{{ cart.product.price * cart.qty }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash @click="deleteCart(cart.product.id)"></b-icon-trash>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Price :</strong>
                  </td>
                  <td align="right">
                    <strong>Rp.{{ totalPrice }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="name">Name</label>
              <input
                type="text"
                id="name"
                class="form-control"
                v-model="order.name"
              />
            </div>
            <div class="form-group">
              <label for="table-number">Table Number</label>
              <input
                type="text"
                id="table-number"
                class="form-control"
                v-model="order.table_number"
              />
            </div>
            <button
              type="submit"
              class="btn btn-success"
              @click="checkoutOrder"
            >
              <b-icon-cart></b-icon-cart> Checkout
            </button>
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
  name: "Cart",
  components: {
    Navbar,
  },
  data() {
    return {
      carts: JSON.parse(localStorage.getItem("cart")),
      order: {},
    };
  },
  methods: {
    deleteCart(id) {
      this.carts = this.carts.filter((val) => val.product.id !== id);
      localStorage.setItem("cart", JSON.stringify(this.carts))
      this.$toast.success("Food has been deleted!", {
        type: "success",
        position: "top-right",
        duration: 3000,
        dismissible: true,
      });
    },
    async checkoutOrder() {
      if (this.order.name && this.order.table_number) {
        this.order.total_price = this.totalPrice;
        this.order.cart = this.carts;

        const response = await axios.post("order", this.order)
          .catch((err) => console.log(err));
        
        if(response.data.meta.code == 200) {
            this.carts = [];
            localStorage.setItem("cart", JSON.stringify(this.carts))

            this.$toast.success("Cart has been order!", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
            
            this.$router.push({ path: "/checkout-success" });
        }

      } else {
        this.$toast.error("Form checkout not be empty!", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    if(this.carts == undefined){
      this.carts = [];
    }
  },
  computed: {
    totalPrice() {
      return this.carts.reduce(function (item, data) {
        return item + data.product.price * data.qty;
      }, 0);
    },
  },
};
</script>

<style scoped>
.img-fluid {
  border-radius: 15px;
}
</style>
