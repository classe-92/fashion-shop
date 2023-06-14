<template>
    <div class="col-12 col-sm-6 col-md-4 col-lg-3">
        <div class="card">
            <div class="card-title">
                <h3>{{ product.name }}</h3>
            </div>
            <div class="card-img-top">
                <img :src="getImagePath" :alt="product.name">
            </div>
            <div class="card-body d-flex justify-content-between align-items-end">
                <div>
                    <h6>{{ product.brand.name }}</h6>
                    <span>{{ product.category.name }}</span>
                </div>
                <div>
                    Price: {{ product.price || 0 }} &euro;
                </div>
                <p class="card-text">
                    <!-- <a href="#" class="btn btn-primary">Go somewhere</a> -->
                    <router-link :to="{ name: 'single-product', params: { slug: product.slug } }"
                        class="btn btn-primary">Vedi prodotto
                    </router-link>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductCard',
    props: ['product'],
    data() {
        return {
            imgBasePath: 'http://127.0.0.1:8000/storage/',
        }
    },
    computed: {
        getImagePath() {
            return this.imgBasePath + this.product.cover_image;
        }
    }
}
</script>

<style lang="scss" scoped></style>