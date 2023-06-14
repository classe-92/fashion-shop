<template>
    <div v-if="product">
        <h1>{{ product.name }}</h1>
        <img :src="imgBasePath + product.cover_image" :alt="product.name">
        <ul>
            <li v-for="color in product.colors" :style="{ backgroundColor: color.hex_value }">
                {{ color.name }}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'SingleProduct',
    data() {
        return {
            product: null,
            apiUrl: 'http://127.0.0.1:8000/api',
            imgBasePath: 'http://127.0.0.1:8000/storage/',
        }
    },
    methods: {
        getProduct() {
            axios.get(`${this.apiUrl}/products/${this.$route.params.slug}`).then((res) => {
                console.log(res.data.results);
                if (res.data.success) {
                    this.product = res.data.results;
                } else {
                    this.$router.push({ name: 'not-found' })
                }
            })
        }
    },
    mounted() {
        // console.log(this.$router);
        // console.log(this.$route);
        this.getProduct();
    }
}
</script>

<style lang="scss" scoped></style>