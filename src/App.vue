<template>
  <h1 class="container ps-5">Formulario</h1>
  <FormularioRegistro @submit-form="submitForm" />
  <TablaRegistro :users="users" />
</template>

<script setup>
import {reactive} from 'vue'
import FormularioRegistro from './components/FormularioRegistro.vue'
import TablaRegistro from './components/TablaRegistro.vue'

const users = reactive([])

const submitForm = (form) => {
  users.push(form)
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
