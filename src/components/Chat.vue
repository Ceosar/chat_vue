<template>
    <article class="chat__container">
        <section ref="messageContainer" class="chat-message__container">
            <div v-if="isStatus200" v-for="(message, index) in messages" :key="index" class="message__content">
                <label class="message__label">{{ message.username }}</label>
                <section class="message__form">
                    <span class="message__text"> {{ message.text }}</span>
                    <span class="message__time">{{ formatTime(message.time) }}</span>
                    <button>
                        <img class="message-delete_btn" src="./../assets/cross_close.svg" alt="">
                    </button>
                </section>
            </div>
            <div v-else class="error_loading">
                <pulse-loader :loading="loading" :color="color" :size="size"></pulse-loader>
                <p>
                    Возникла ошибка
                </p>
            </div>
        </section>
        <section class="chat-sendform__container">
            <div class="chat-sendform__content">
                <input v-model="inputValue" class="chat-sendform__input" type="text" placeholder="Сообщение">
                <button v-if="inputValue" @click="sendMessage">
                    <img class="chat-sendform__btn" src="./../assets/send_button.svg" />
                </button>
            </div>
        </section>
    </article>
</template>


<script>
import axios from "axios";
import { username } from "./Login.vue"
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

export default {
    components: {
        PulseLoader
    },
    data() {
        return {
            inputValue: '',
            currentUser: '',
            messages: [],
            isStatus200: false,
        };
    },
    methods: {
        sendMessage() {
            axios.post("http://firstphp/index.php", {
                text: this.inputValue,
                username: username.value,
            })
                .then(response => {
                    this.isStatus200 = true;
                    console.log(response);
                    this.inputValue = "";

                    this.$nextTick(() => {
                        this.$refs.messageContainer.scrollTop = this.$refs.messageContainer.scrollHeight;
                    })
                })
                .catch(error => {
                    this.isStatus200 = false;
                    console.log(error);
                })
                .finally(
                    () => {
                        this.getLastMessage();
                    }
                )
        },
        getMessage() {
            axios.get('http://firstphp/get_message.php')
                .then(response => {
                    this.isStatus200 = true;
                    response.data.reverse().forEach(element => {
                        this.messages.push(element);
                    });
                    this.$nextTick(() => {
                        this.$refs.messageContainer.scrollTop = this.$refs.messageContainer.scrollHeight;
                    })

                })
                .catch(error => {
                    this.isStatus200 = false;
                    console.log(error);
                })
        },
        getLastMessage() {
            axios.get('http://firstphp/get_message.php')
                .then(response => {
                    this.isStatus200 = true;
                    this.messages.push(response.data[0]);
                    this.$nextTick(() => {
                        this.$refs.messageContainer.scrollTop = this.$refs.messageContainer.scrollHeight;
                    })

                })
                .catch(error => {
                    this.isStatus200 = false;
                    console.log(error);
                })
        },
        formatTime(timeString) {
        const time = new Date(timeString);
        const hours = time.getHours();
        const minutes = time.getMinutes();
        const formattedHours = hours < 10 ? '0' + hours : hours;
        const formattedMinutes = minutes < 10 ? '0' + minutes : minutes;
        return formattedHours + ':' + formattedMinutes;
    }
    },
    beforeMount() {
        this.getMessage()
    }
}
</script>


<style scoped>
.chat__container {
    width: 570px;
    max-height: 630px;
    border: 1px solid rgba(35, 44, 94, 0.21);
    border-radius: 15px;
    background-color: rgba(245, 245, 248, 1);
    -webkit-box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
    -moz-box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
    box-shadow: 1px 1px 8px 7px rgba(34, 60, 80, 0.11);
}

.chat-message__container {
    padding: 20px;
    height: 470px;
    background-color: white;
    margin-top: 30px;
    border-top: 1px solid rgba(35, 44, 94, 0.21);
    border-bottom: 1px solid rgba(35, 44, 94, 0.21);
    overflow: auto;
    display: flex;
    flex-direction: column;
    row-gap: 24px;
}

.chat-message__container::-webkit-scrollbar {
    width: 5px;
}

.chat-message__container::-webkit-scrollbar-thumb {
    background-color: rgba(35, 44, 94, 0.12);
    border-radius: 5px;
}

.chat-message__container::-webkit-scrollbar-track {
    background-color: transparent;
}

.message__content {
    display: flex;
    flex-direction: column;
    row-gap: 5px;
    position: relative;
    cursor: pointer;
}

.message__label {
    color: rgba(94, 94, 94, 1);
    font-size: 16px;
    font-weight: 400;
}

.message__form {
    width: 70%;
    padding: 20px;
    background-color: rgba(245, 245, 248, 1);
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    position: relative;
}

.message__text {
    color: rgba(34, 34, 34, 1);
    font-size: 16px;
    font-weight: 400;
}

.message__time {
    color: rgba(150, 150, 150, 1);
    font-size: 13px;
    font-weight: 400;
    position: absolute;
    bottom: 6px;
    right: 6px;
}

.message-delete_btn {
    opacity: 0;
    width: 18px;
    height: 18px;
    position: absolute;
    right: 6px;
    top: 6px;
    transition: all .4s;
}

.message-delete_btn:hover {
    opacity: 1;
}

.chat-sendform__container {
    padding: 40px 0;
    display: flex;
    justify-content: center;
}

.chat-sendform__content {
    display: flex;
    align-items: center;
    justify-content: space-around;
    width: 90%;
    padding: 18px;
    height: 50px;
    border: 1px solid rgba(35, 44, 94, 0.21);
    border-radius: 8px;
    background-color: white;
}

.chat-sendform__input {
    width: 100%;
    color: rgba(34, 34, 34, 1);
}

.chat-sendform__btn {
    cursor: pointer;
}

.error_loading {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10%;
}

.error_loading p{
    font-size: 25px;
    font-weight: 400;
}
</style>