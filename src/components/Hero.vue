<template>
    <div class="hero">
        
        <!-- Desktop -->
        <div class="d-none d-md-block">
            <div class="row mt-4">
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Delicious Food Menu,</strong><br> In Your Gadget</h2>
                            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
                            <router-link to="/foods" class="btn btn-lg btn-success"><b-icon-arrow-right></b-icon-arrow-right> Find Foods</router-link>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <img src="../assets/images/ilustrasi.png" alt="Hero" width="100%" />
                </div>
            </div>
        </div>

        <!-- mobile -->
        <div class="d-sm-block d-md-none">
            <div class="row mt-4">
                <div class="col-md-6 mb-3">
                    <img src="../assets/images/ilustrasi.png" alt="Hero" width="100%" />
                </div>
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Delicious Food Menu,</strong><br> In Your Gadget</h2>
                            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
                            <button class="btn btn-lg btn-success"><b-icon-arrow-right></b-icon-arrow-right> Find Foods</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Hero"
}
</script>

<style scoped>

</style>
