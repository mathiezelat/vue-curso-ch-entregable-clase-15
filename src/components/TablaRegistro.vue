<template>
    <div class="container ps-5" v-if="users.length > 0">
        <table class="table">
        <thead>
            <tr>
            <th scope="col">Nombre</th>
            <th scope="col">Apellido</th>
            <th scope="col">Edad</th>
            <th scope="col">Correo electrónico</th>
            <th scope="col">País</th>
            <th scope="col">Cursos</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="user in users" :key="user.email">
            <td>{{user.nombre}}</td>
            <td>{{user.apellido}}</td>
            <td>{{user.edad}}</td>
            <td>{{user.email}}</td>
            <td>{{user.pais}}</td>
            <td>{{user.cursos.join(', ')}}</td>
            </tr>
        </tbody>
        </table>
    </div>
</template>

<script setup>
import { defineProps } from 'vue'

defineProps({
    users: Array
})

</script>

<style>

</style>