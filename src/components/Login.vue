<script>
import { ref } from 'vue';

export const username = ref('Anonim');

export default {
    methods: {
        inputChange(event) {
            if (event.target.value == '') {
                username.value = "Anonim";
            } else {
                username.value = event.target.value;
            }
        }
    }
}
</script>

<template>
    <article class="login__container">
        <section class="login__content">
            <span class="login__label">Введите логин</span>
            <input v-model="username" @input="inputChange" class="login__input" type="text" placeholder="Логин">
        </section>
    </article>
</template>

<style scoped>
.login__container {
    width: 370px;
    height: 170px;
    border: 1px solid rgba(35, 44, 94, 0.21);
    border-radius: 15px;
    background-color: rgba(255, 255, 255, 1);
    -webkit-box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
    -moz-box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
    box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
}

.login__content {
    margin: 0 auto;
    width: 90%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    row-gap: 6px;
}

.login__label {
    font-size: 16px;
    font-weight: 400;
    color: rgba(94, 94, 94, 1);
}

.login__input {
    padding: 18px;
    font-size: 16px;
    font-weight: 400;
    color: rgba(34, 34, 34, 1);
    border: 1px solid rgba(35, 44, 94, 0.21);
    border-radius: 8px;
    transition: all .3s;
}

.login__input:focus {
    border: 1px solid rgba(35, 44, 94, 1);
}
</style>