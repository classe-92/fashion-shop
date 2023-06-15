<template>
    <LoaderApp v-if="loading" />
    <section class="container" v-if="!loading">
        <div class="mb-4">
            <div class="container-fluid d-flex justify-content-between">
                <div class="p-4 w-25">
                    <img :src="getImagePath" :alt="product.name" class="image-fit" />
                </div>

                <div class="p-4 w-75">
                    <h1 class="display-5 fw-bold">{{ product.name }}</h1>
                    <p class="fs-4">{{ product.description }}
                    </p>
                    <button class="btn btn-primary btn-lg" type="button"><i class="fa-solid fa-cart-shopping"></i>
                        <span> Add to Cart</span>
                    </button>
                </div>
                <router-link :to="{ name: 'single-product', params: { slug: 'blotted-lip' } }" class="btn btn-primary">Altro
                    prodotto
                </router-link>

            </div>
        </div>
        <ul v-if="product.colors">
            <li v-for="color in product.colors" :style="{ backgroundColor: color.hex_value }">
                {{ color.name }}
            </li>
        </ul>
    </section>
</template>

<script>
import { store } from '../store';
import LoaderApp from '../components/LoaderApp.vue';
import axios from 'axios'
export default {
    name: 'SingleProduct',
    components: {
        LoaderApp
    },
    data() {
        return {
            store,
            product: null,
            loading: true
        }
    },
    methods: {
        getProduct() {
            axios.get(`${store.apiURL}/products/${this.$route.params.slug}`).then((res) => {
                console.log(res.data.results);
                this.product = res.data.results;
            }).catch((error) => {
                console.log(error);
                console.log(error.response.data);
                this.$router.push({ name: 'not-found', query: { e: error.response.data.message } });
            }).finally(() => {
                this.loading = false;
            });
        }
    },
    computed: {
        getImagePath() {
            return store.imgBasePath + this.product.cover_image;
        }
    },
    mounted() {
        // console.log(this.$router);
        // console.log(this.$route);

        this.getProduct();
    },
    created() {
        this.$watch(
            () => this.$route.params,
            (toParams, previousParams) => {
                // react to route changes...
                this.getProduct();
            }
        )
    },
}
</script>

<style lang="scss" scoped></style>