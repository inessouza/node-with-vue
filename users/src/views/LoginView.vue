<template>
    <div>
        <h2>Login</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <p>E-mail</p>
                <input type="email" placeholder="email@email.com" class="input" v-model="email">
                <p>Senha</p>
                <input type="password" placeholder="*********" class="input" v-model="password"><hr>
                <button class="button is-success" @click="login">Logar</button><hr><hr>
                <div class="notification is-danger" v-if="err != undefined">
                    <p>{{ err }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            password: '',
            email: '',
            err: undefined
        }
    },
    methods: {
        login() {
            axios.post('http://localhost:8686/login', {
                password: this.password,
                email: this.email
            }).then(res => {
                console.log(res)
                localStorage.setItem('token', res.data.token)
                this.$router.push({name: 'home'})
            }).catch(err => {
                var msgErro = err.response.data.err
                this.err = msgErro
            })
        }
    }
}

</script>

<style>

</style>
