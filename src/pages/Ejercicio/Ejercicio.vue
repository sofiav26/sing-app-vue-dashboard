<template>
  <div>
    <h1 class="page-title">Ejercicio</h1>
    <b-button variant="info" class="mr-2 mb-2" v-on:click="(filter = 1)">Borrador</b-button>
    <b-button variant="info" class="mr-2 mb-2" v-on:click="(filter = 2)">Confirmadas</b-button>
    <b-button variant="info" class="mb-2" v-on:click="(filter = 3)">Visadas</b-button>
    <b-row>
      <b-col xs="6" lg="3">
        <b-card-group deck v-bind:key="user.id" v-for="user of userList">
          <b-card v-if="(user.state.id == filter)"
          :name="user.name"
          :id="user.id"
          :date="user.date"
          :address="user.address"
          :stateId="user.state.id"
          :stateName="user.state.name"
          :cep="user.cep"
          :timbrado="user.timbrado"
          class="mb-2"
          >
              <p class="name">{{ user.name }}</p>
              <div class="row">
                  <div class="col-3">
                      <b-badge variant="info">{{ user.id }}</b-badge>
                  </div>
                  <div class="col-4">
                  <p>{{ user.date }}</p>
                  </div>
              </div>
              <p class="address">{{ user.address }}</p>
              <hr/>
              <div v-if="(filter == 1)">
                <p class="ver text-info">VER FICHA</p>
              </div>
              <div v-if="(filter == 2)">
                <div class="row ml-2">
                  <div class="col-5">
                    <div class="row">
                      <p class="timbrado text-info mr-2 mt-2">TIMBRADO:</p>
                      <i v-if="(user.timbrado)" class="glyphicon glyphicon-ok-sign mt-2"></i>
                      <b-button v-if="(!user.timbrado)" @click="pagarTimbrado" variant="info">PAGAR</b-button>  
                    </div>
                  </div>
                  <div class="col-5 ml-2">
                    <div class="row">
                      <p class="timbrado text-info mr-2 mt-2">CEP:</p>
                      <i v-if="(user.cep)" class="glyphicon glyphicon-ok-sign mt-2"></i>
                      <b-button v-if="(!user.cep)" @click="pagarCep" variant="info">PAGAR</b-button>
                    </div>
                  </div>
                </div>
              </div>
              <div v-if="(filter == 3)">
                <b-dropdown class="dropdownVisadas" variant="outline-inverse">
                  <b-dropdown-item>Detalle Ficha</b-dropdown-item>
                  <b-dropdown-item>Agregar Ampliación</b-dropdown-item>
                  <b-dropdown-item>Agregar Certificado</b-dropdown-item>
                </b-dropdown>
              </div>
          </b-card>
        </b-card-group>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import Widget from '@/components/Widget/Widget';
import { BCard } from 'bootstrap-vue';
import axios from 'axios';

export default {
  name: 'Ejercicio',
  components: { Widget, BCard },
  data() {
    return {
      filter: 0,
      userList: this.users,
      loading: false,
      text: 'Detalle Ficha',
    };
  },
  methods: {
    fetchUsers() {
      this.loading = true;
      axios
        .get("http://mock.freelogic.com.ar/res/76/id/optional_uri",
        {
          mode:'no-cors',
        })
        .then((response) => {
          this.userList = response.data;
          console.log("data:", response.data);
        });
    },
  },
  beforeMount: function(){
    this.fetchUsers();
  },
};

</script>
<style src="./Ejercicio.scss" lang="scss" />
