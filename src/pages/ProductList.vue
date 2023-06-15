<template>
    <LoaderApp v-if="loading" />
    <section class="container" id="product-list" v-if="!loading">
        <div class="d-flex justify-content-between mb-3">
            <h1>{{ title }}</h1>

            <select name="category" id="category" v-model="selectedCategory" @change="getData(1)">
                <option value="">All</option>
                <option :value="category.id" v-for="(category, index) in categories" :key="category.id">{{ category.name }}
                </option>
            </select>

        </div>

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
            categories: [],
            selectedCategory: '',
            currentPage: 1,
            lastPage: null,
            loading: true
        }
    },
    methods: {
        getData(numPage) {
            let params = {
                'page': numPage
            }
            if (this.selectedCategory) {
                params.category_id = this.selectedCategory
            }
            axios.get(`${store.apiURL}/products`, {
                params

            }).then((res) => {
                //console.log(res);
                this.products = res.data.results.products.data;
                this.currentPage = res.data.results.products.current_page;
                this.lastPage = res.data.results.products.last_page;
                this.categories = res.data.results.categories;
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