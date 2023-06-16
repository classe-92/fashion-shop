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
        <div class="row justify-content-center my-4">
            <div class="col-12 col-lg-8">
                <div id="productCarousel" class="carousel carousel-dark slide">
                    <div class="carousel-indicators">
                        <button type="button" v-for="(product, index) in products" :key="product.id"
                            data-bs-target="#productCarousel" :class="{ 'active': activeSlide === index }">
                        </button>
                    </div>
                    <div class="carousel-inner">
                        <Transition :name="direction">
                            <div class="carousel-item active" :key="products[activeSlide].id">
                                <img :src="store.imgBasePath + products[activeSlide].cover_image" class="d-block"
                                    :alt="products[activeSlide].name" />


                                <div class="carousel-caption d-none d-md-block">
                                    <h5>{{ products[activeSlide].name }}</h5>
                                    <!-- <p>{{ product.descrption }}</p> -->
                                </div>
                            </div>
                        </Transition>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#productCarouselCaptions"
                        data-bs-slide="prev" @click="prev()">
                        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="##productCarouselCaptions"
                        data-bs-slide="next" @click="next()">
                        <span class="carousel-control-next-icon" aria-hidden="true"></span>
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>

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
            categories: [],
            activeSlide: 0,
            direction: 'next'
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
        },
        next() {
            this.direction = 'next';
            this.activeSlide = this.activeSlide < this.products.length - 1 ? this.activeSlide + 1 : 0;
        },
        prev() {
            this.direction = 'prev';
            this.activeSlide = this.activeSlide > 0 ? this.activeSlide - 1 : this.products.length - 1;
        }
    },
    mounted() {
        this.getData();
    }
}
</script>

<style lang="scss" scoped></style>
