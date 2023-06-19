<template>
    <section class="container m-4">
        <h2 class="text-uppercase">contact me</h2>
        <div v-if="success" class="alert alert-success text-start" role="alert">
            Messaggio inviato con successo!
        </div>
        <div class="row">
            <form @submit.prevent="sendForm()" class="col-12 text-start">
                <div class="mb-3">
                    <input class="border-0 border-bottom form-control" :class="{ 'is-invalid': errors.name }" type="text"
                        name="name" id="name" placeholder="Name" v-model="name">
                    <p v-for="(error, index) in errors.name" :key="`message-error-${index}`" class="invalid-feedback">
                        {{ error }}
                    </p>
                </div>
                <div class="mb-3">
                    <input class="border-0 border-bottom form-control" :class="{ 'is-invalid': errors.email }" type="text"
                        name="address" id="address" placeholder="Email" v-model="address">
                    <p v-for="(error, index) in errors.email" :key="`message-error-${index}`" class="invalid-feedback">
                        {{ error }}
                    </p>
                </div>
                <div class="mb-3">
                    <textarea class="border-0 border-bottom form-control" :class="{ 'is-invalid': errors.message }"
                        name="message" id="message" cols="30" rows="10" placeholder="Message" v-model="message"></textarea>
                    <p v-for="(error, index) in errors.message" :key="`message-error-${index}`" class="invalid-feedback">
                        {{ error }}
                    </p>
                </div>
                <button class="btn btn-lg btn-primary text-white" type="submit" :disabled="loading">{{ loading ?
                    'Sending...' : 'Send'
                }}</button>
            </form>
        </div>

    </section>
</template>

<script>
import axios from 'axios';
import { store } from '../store';
export default {
    name: 'ContactUs',

    data() {
        return {
            store,
            name: '',
            address: '',
            message: '',
            loading: false,
            success: false,
            errors: {}
        }
    },
    methods: {
        sendForm() {
            this.loading = true;
            const data = {
                name: this.name,
                address: this.address,
                message: this.message
            };

            // pulisco l'array con i messaggi
            this.errors = {};

            // Importante - Stiamo comunicando con Laravel, quindi non è più obbligatorio inserire gli headers con il Content-Type
            // come abbiamo fatto invece quando comunicavamo direttamente con gli script PHP
            axios.post(`${this.store.apiURL}/contacts`, data).then((response) => {
                this.success = response.data.success;
                if (!this.success) {
                    this.errors = response.data.errors;
                } else {
                    // ripulisco i campi di input
                    this.name = '';
                    this.address = '';
                    this.message = '';
                }
                this.loading = false;
            });
        },
    }
}

</script>

<style lang="scss" scoped></style>