<template>
    <div>
        <h2>Painel Administrativo</h2>
        <div class="columns is-centered mt-5">
            <table class="table">
                <thead>
                    <th>Nome</th>
                    <th>Email</th>
                    <th>Cargo</th>
                    <th>Ações</th>
                </thead>
                <tbody>
                    <tr v-for="user in users" :key="user.id">
                        <td>{{ user.name }}</td>
                        <td>{{ user.email }}</td>
                        <td>{{ user.role === 0 ? 'Usuário Comum' : 'Admin' }}</td>
                        <td>
                            <router-link :to="{name: 'UserEdit', params: {id: user.id}}"><button class="button is-success">Editar</button></router-link>  |
                            <button class="button is-danger" @click="showModalUser(user.id)">Deletar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
            <div :class="{modal: true, 'is-active': showModal}">
              <div class="modal-background"></div>
              <div class="modal-content">
                <div class="card">
                  <header class="card-header">
                    <p class="card-header-title is-centered">
                      Você realmente quer deletar este usuário?
                    </p>
                    <button class="card-header-icon" aria-label="more options">
                      <span class="icon">
                        <i class="fas fa-angle-down" aria-hidden="true"></i>
                      </span>
                    </button>
                  </header>
                  <p>
                    Esta ação não pode ser desfeita!
                  </p>
                  <footer class="card-footer">
                    <a href="#" class="card-footer-item" @click="hideModal()">Cancelar</a>
                    <a href="#" class="card-footer-item" @click="deleteUser()">Deletar</a>
                  </footer>
                </div>
              </div>
              <button class="modal-close is-large" aria-label="close" @click="hideModal()"></button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    created() {
        var req = {
          headers: {
            Authorization: "Bearer " + localStorage.getItem('token')
          }
        }
        axios.get("http://localhost:8686/user", req).then(res => {
            console.log(res)
            this.users = res.data
        }).catch(err => {
            console.log(err)
        })
        console.log("olá")
    },
    data() {
        return {
            users: [],
            showModal: false,
            deleteUserId: -1
        }
    },
    methods: {
        hideModal() {
            this.showModal = false
        },
        showModalUser(id) {
            this.deleteUserId = id
            this.showModal = true
        },
        deleteUser() {
            var req = {
              headers: {
                Authorization: "Bearer " + localStorage.getItem('token')
              }
            }
            axios.delete("http://localhost:8686/user/"+this.deleteUserId, req).then(res => {
                console.log(res)
                this.showModal = false
                this.users = this.users.filter(u => u.id != this.deleteUserId)
            }).catch(err => {
                console.log(err)
                this.showModal = false
            })
        }
    }
}

</script>

<style>

</style>
