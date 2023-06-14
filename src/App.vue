<template>
    <HeaderApp />
    <main>
        <router-view></router-view>
    </main>

    <footer>
        <h3>Sono Il footer</h3>
    </footer>
</template>

<script>
import HeaderApp from './components/HeaderApp.vue';
export default {
    name: 'App',
    components: {
        HeaderApp
    }

}
</script>

<style lang="scss" scoped></style>
