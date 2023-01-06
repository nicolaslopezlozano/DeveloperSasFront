<template>
    <div>
        <div class="container">
            <input
                v-model="buscar_campo"
                class="form-control form-control-sm"
            >
            <button class="btn btn-primary btn-sm" type="button" @click="buscar">
                Buscar
            </button>
            <br>
            <table class="table table-striped">
                <thead class="text-center">
                    <tr>
                      <th scope="col">Nombre</th>
                      <th scope="col">Email</th>
                      <th scope="col">Cargo</th>
                      <th scope="col">Salario</th>
                    </tr>
                </thead>
                <tbody class="text-center">
                    <tr v-for="item in users" :key="item.id">
                        <td>{{ item.name }}</td>
                         <td>{{ item.email }}</td>
                         <td>{{ item.position }}</td>
                         <td>{{ item.salary }}</td>
                        <td>
                          <button
                              type="button"
                              class="btn btn-primary"
                              @click="abrirModal(item)"
                              >
                              Detalle
                          </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div
            class="modal"
            :class="{show_modal:modal}"
            tabindex="-1"
            aria-labelledby="createUserModalLabel"
            aria-hidden="true"
            >
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h1 class="modal-title fs-5" id="exampleModalLabel">Detalle</h1>
                  <button type="button" @click="cerrarModal()" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    Salario: {{ this.salary }} <br>
                    Nombre: {{ this.name }} <br>
                    Telefono: {{ this.phone }} <br>
                    Correo: {{ this.email }} <br>
                    Cargo: {{ this.position }} <br>
                    Phone: {{ this.phone }} <br>
                    Salario: {{ this.salary }} <br>
                    Dirección: {{ this.address }} <br>
                    Habilidades: {{ this.skills }} <br>
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="cerrarModal()">Close</button>
                </div>
              </div>
            </div>
        </div>

    </div>
</template>

<script>
const axios = require('axios');
import router from '@/router';
export default {
    name: 'Diary',
    data() {
        return {
            users: [],
            modal: 0, 
            name: null,
            phone: null,
            email: null,
            position: null,
            phone: null,
            salary:null,
            address: null,
            skills: null,
            buscar_campo: null
        }
    },
    mounted() {
        this.getUsers();
    },
    methods: {
        getUsers(){
            axios
                .get('http://127.0.0.1:8000/api/lista-empleados/')
                .then(response => {
                    this.users = response.data;
                });
        },
        abrirModal(item){
          this.modal = 1;
          this.salary = item.salary;
          this.name = item.name;
          this.phone = item.phone;
          this.email = item.email;
          this.position = item.position;
          this.phone = item.phone;
          this.salary = item.salary;
          this.address = item.address;
          this.skills = item.skills;
        },
        cerrarModal(){
          this.modal = 0;
        },
        buscar(){
          axios
            .get('http://127.0.0.1:8000/api/email-empleados/'+this.buscar_campo)
            .then(response => {
                this.users = null;
                this.users = response.data;
            });
        }
    },
}
</script>

<style scoped>
.show_modal{
    display: list-item;
    opacity:1;
    background: rgba(65, 62, 62, 0.5);
  }
</style>