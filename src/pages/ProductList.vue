<template>
    <div class="container" id="product-list">
        <h1>{{ title }}</h1>
        <div class="row gy-4 mb-4">
            <ProductCard v-for="(product, index) in products" :key="product.id" :product="product" />

        </div>
    </div>
    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li class="page-item"><button :class="{ 'page-link': true, 'disabled': currentPage === 1 }"
                    @click="getData(currentPage - 1)">Previous</button></li>
            <li class="page-item" v-for="n in lastPage"><button :class="{ 'page-link': true, 'active': currentPage === n }"
                    @click="getData(n)">{{ n }}</button>
            </li>

            <li class="page-item"><button :class="{ 'page-link': true, 'disabled': currentPage === lastPage }"
                    @click="getData(currentPage + 1)">Next</button></li>
        </ul>
    </nav>
</template>

<script>
import axios from 'axios';
import ProductCard from '../components/ProductCard.vue';
export default {
    'name': 'ProductList',
    components: {
        ProductCard
    },
    data() {
        return {
            title: 'Products List',
            products: [],
            apiUrl: 'http://127.0.0.1:8000/api',
            imgBasePath: 'http://127.0.0.1:8000/storage/',
            currentPage: 1,
            lastPage: null,
        }
    },
    methods: {
        getData(numPage) {
            axios.get(`${this.apiUrl}/products`, {
                params: {
                    'page': numPage
                }
            }).then((res) => {
                //console.log(res);
                this.products = res.data.results.data;
                this.currentPage = res.data.results.current_page;
                this.lastPage = res.data.results.last_page;
            })
        }
    },
    mounted() {
        this.getData(1);
    }

}
</script>

<style lang="scss" scoped></style>