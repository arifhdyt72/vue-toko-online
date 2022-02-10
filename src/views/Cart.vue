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
                <tr v-for="(cart, index) in carts" :key="cart.id">
                  <td>{{ index + 1 }}</td>
                  <td>
                    <img
                      :src="'../assets/images/' + cart.product.gambar"
                      :alt="cart.product.nama"
                      class="img-fluid shadow"
                      width="200"
                    />
                  </td>
                  <td>
                    <strong>{{ cart.product.nama }}</strong>
                  </td>
                  <td>{{ cart.desc ? cart.desc : "-" }}</td>
                  <td>{{ cart.qty }}</td>
                  <td align="right">
                    <strong>Rp.{{ cart.product.harga }}</strong>
                  </td>
                  <td align="right">
                    <strong>Rp.{{ cart.product.harga * cart.qty }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash @click="deleteCart(cart.id)"></b-icon-trash>
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
                v-model="order.tableNumber"
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
      carts: [],
      order: {},
    };
  },
  methods: {
    setCarts(data) {
      this.carts = data;
    },
    deleteCart(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.success("Food has been deleted!", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          //update cart
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setCarts(response.data))
            .catch((error) => console.log("Gagal: ", error));
        })
        .catch((error) => console.log("Gagal: ", error));
    },
    checkoutOrder() {
      if (this.order.name && this.order.tableNumber) {
        this.order.total_price = this.totalPrice;
        this.order.cart = this.carts;

        axios
          .post("http://localhost:3000/pesanans", this.order)
          .then(() => {
            this.carts.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log("Gagal: ", error));
            });

            this.$router.push({ path: "/checkout-success" });
            this.$toast.success("Cart has been order!", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
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
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setCarts(response.data))
      .catch((error) => console.log("Gagal: ", error));
  },
  computed: {
    totalPrice() {
      return this.carts.reduce(function (item, data) {
        return item + data.product.harga * data.qty;
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
