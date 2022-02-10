<template>
    <div class="card shadow card-product">
        <img :src="'assets/images/'+product.gambar" class="card-img-top" :alt="product.nama" />
        <div class="card-body">
            <h5 class="card-title">{{product.nama}}</h5>
            <p class="card-text">Harga Rp. {{product.harga}}</p>
            <router-link :to="'foods/'+product.id" class="btn btn-success"><b-icon-cart></b-icon-cart> Checkout</router-link>
        </div>
    </div>
</template>

<script>

export default {
    name: "CardProduct",
    props: ["product"]
}

</script>

<style scoped>
    .card-product{
        border-radius: 15px;
    }
    .card-img-top{
        border-top-left-radius: 15px;
        border-top-right-radius: 15px;
        height: 170px;
    }
</style>
