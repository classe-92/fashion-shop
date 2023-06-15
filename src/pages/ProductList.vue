<template>
    <LoaderApp v-if="loading" />
    <section class="container" id="product-list" v-if="!loading">
        <h1>{{ title }}</h1>
        <div class="row gy-4 mb-4">
            <ProductCard v-for="(product, index) in products" :key="product.id" :product="product" />
        </div>
        <nav aria-label="Page navigation example">
            <ul class="pagination">
                <li class="page-item"><button :class="{ 'page-link': true, 'disabled': currentPage === 1 }"
                        @click="getData(currentPage - 1)">Previous</button></li>
                <li class="page-item" v-for="n in lastPage"><button
                        :class="{ 'page-link': true, 'active': currentPage === n }" @click="getData(n)">{{ n }}</button>
                </li>

                <li class="page-item"><button :class="{ 'page-link': true, 'disabled': currentPage === lastPage }"
                        @click="getData(currentPage + 1)">Next</button></li>
            </ul>
        </nav>
    </section>
</template>

<script>
import { store } from '../store';
import axios from 'axios';
import ProductCard from '../components/ProductCard.vue';
import LoaderApp from '../components/LoaderApp.vue';
export default {
    'name': 'ProductList',
    components: {
        ProductCard,
        LoaderApp
    },
    data() {
        return {
            store,
            title: 'Products List',
            products: [],
            currentPage: 1,
            lastPage: null,
            loading: true
        }
    },
    methods: {
        getData(numPage) {
            axios.get(`${store.apiURL}/products`, {
                params: {
                    'page': numPage
                }
            }).then((res) => {
                //console.log(res);
                this.products = res.data.results.data;
                this.currentPage = res.data.results.current_page;
                this.lastPage = res.data.results.last_page;
            }).catch((error) => {
                console.log(error);
            }).finally(() => {
                this.loading = false;
            })
        }
    },
    mounted() {
        this.getData(1);
    }

}
</script>

<style lang="scss" scoped></style>