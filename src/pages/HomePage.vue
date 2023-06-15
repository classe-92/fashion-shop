<template>
    <LoaderApp v-if="loading" />
    <section class="hero">
        <h1>MyColors</h1>
    </section>
    <section class="container my-4" v-if="!loading">
        <div class="row gy-4 gx-1 justify-content-center">
            <div class="col-2 col-md-2 col-lg-1" v-for="(category, index) in categories" :key="category.id">
                <div class="card">

                    <div class="card-img-top h-300 p-3">
                        <img :src="'/images/' + category.slug + '.svg'" :alt="category.name" class="image-fit">
                    </div>
                    <div class="card-title">
                        <h6 class=" text-center">{{ category.name }}</h6>
                    </div>
                </div>

            </div>

        </div>
        <!-- <div id="carouselExampleCaptions" class="carousel slide">
            <div class="carousel-indicators">
                <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" class="active"
                    aria-current="true" aria-label="Slide 1"></button>
                <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1"
                    aria-label="Slide 2"></button>
                <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2"
                    aria-label="Slide 3"></button>
            </div>
            <div class="carousel-inner">
                <div class="carousel-item active" v-for="">
                    <img :src="store.imgBasePath + products[0].cover_image" class="d-block w-100" alt="...">
                    <div class="carousel-caption d-none d-md-block">
                        <h5>First slide label</h5>
                        <p>Some representative placeholder content for the first slide.</p>
                    </div>
                </div>
            </div>
            <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions"
                data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
            </button>
            <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions"
                data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
            </button>
        </div> -->
    </section>
</template>

<script>
import { store } from '../store';
import axios from 'axios';
import LoaderApp from '../components/LoaderApp.vue';
export default {
    name: 'HomePage',
    components: {
        LoaderApp
    },
    data() {
        return {
            store,
            loading: true,
            products: [],
            categories: []
        }
    },
    methods: {
        getData() {
            axios.get(`${store.apiURL}/mixed`).then((res) => {
                console.log(res.data.results);
                this.products = res.data.results.products;
                this.categories = res.data.results.categories;
            }).catch((errors) => {
                console.log(errors);
            }).finally(() => {
                this.loading = false;
            });
        }
    },
    mounted() {
        this.getData();
    }
}
</script>

<style lang="scss" scoped></style>