<template>
    <div>
        <h2>Registro de Usuário</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <p>Nome</p>
                <input type="text" placeholder="Nome do Usuário" class="input" v-model="name">
                <p>E-mail</p>
                <input type="email" placeholder="email@email.com" class="input" v-model="email">
                <p>Senha</p>
                <input type="password" placeholder="*********" class="input" v-model="password"><hr>
                <button class="button is-success" @click="register">Cadastrar</button><hr><hr>
                <div v-if="err != undefined">
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
            name: '',
            password: '',
            email: '',
            err: undefined
        }
    },
    methods: {
        register() {
            axios.post('http://localhost:8686/user', {
                name: this.name,
                password: this.password,
                email: this.email
            }).then(res => {
                console.log(res)
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
