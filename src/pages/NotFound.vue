<template>
    <section class="container">
        <h1>OOOPS ! Errore</h1>
        {{ $route.query.e }}
    </section>
</template>

<script>
export default {
    name: 'NotFound',

}
</script>

<style lang="scss" scoped></style>