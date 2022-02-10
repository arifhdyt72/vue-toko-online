<template>
    <div class="checkout-success">
        <Navbar />
        <div class="container">
            <div class="row justify-content-center">
                <div class="col text-center">
                    <img src="../assets/images/menunggu.png" alt="Waiting Order" width="370" />
                    <h2><strong>Success!!</strong></h2>
                    <h4>Your order has been successfully processed<br/>please wait...</h4>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
    name: 'CheckoutSuccess',
    components:{
        Navbar
    }
}
</script>

<style scoped>

</style>
