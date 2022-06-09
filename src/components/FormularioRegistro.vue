<template>
  <form class="container p-5" @submit.prevent="submitForm">
    <div class="mb-3">
      <label for="nombre" class="form-label">Nombre</label>
      <input
        type="text"
        class="form-control"
        id="nombre"
        v-model.trim="v$.form.nombre.$model"
      />
      <div v-if="v$.form.nombre.$dirty">
        <small class="text-danger" v-if="v$.form.nombre.required.$invalid"
          >Nombre es requerido</small
        >
        <small class="text-danger" v-if="v$.form.nombre.minLength.$invalid">
          Nombre tiene que tener al menos
          {{ v$.form.nombre.minLength.$params.min }} caracteres
        </small>
        <small class="text-danger" v-if="v$.form.nombre.alpha.$invalid"
          >Nombre no puede contener números</small
        >
      </div>
    </div>
    <div class="mb-3">
      <label for="apellido" class="form-label">Apellido</label>
      <input
        type="text"
        class="form-control"
        id="apellido"
        v-model.trim="v$.form.apellido.$model"
      />
      <div v-if="v$.form.apellido.$dirty">
        <small class="text-danger" v-if="v$.form.apellido.required.$invalid"
          >Apellido es requerido</small
        >
        <small class="text-danger" v-if="v$.form.apellido.minLength.$invalid">
          Apellido tiene que tener al menos
          {{ v$.form.apellido.minLength.$params.min }} caracteres
        </small>
        <small class="text-danger" v-if="v$.form.apellido.alpha.$invalid"
          >Apellido no puede contener números</small
        >
      </div>
    </div>
    <div class="mb-3">
      <label for="edad" class="form-label">Edad</label>
      <input
        type="number"
        class="form-control"
        id="edad"
        v-model.trim="v$.form.edad.$model"
      />
      <div v-if="v$.form.edad.$dirty">
        <small class="text-danger" v-if="v$.form.edad.required.$invalid"
          >Edad es requerido</small
        >
        <small class="text-danger" v-if="v$.form.edad.between.$invalid"
          >Edad tiene que ser mayor a 18</small
        >
        <small class="text-danger" v-if="v$.form.edad.integer.$invalid"
          >Edad tiene que ser un número entero</small
        >
      </div>
    </div>
    <div class="mb-3">
      <label for="email" class="form-label">Correo electrónico</label>
      <input
        type="email"
        class="form-control"
        id="email"
        v-model.trim="v$.form.email.$model"
      />
      <div v-if="v$.form.email.$dirty">
        <small class="text-danger" v-if="v$.form.email.required.$invalid"
          >Correo electrónico es requerido</small
        >
        <small class="text-danger" v-if="v$.form.email.email.$invalid"
          >Correo electrónico es inválido</small
        >
      </div>
    </div>
    <div class="mb-3">
      <label class="form-label" for="pais"> Pais </label>
      <select class="form-select" id="pais" v-model="v$.form.pais.$model">
        <option v-for="pais in listaPaises" :key="pais">
          {{ pais }}
        </option>
      </select>
      <div v-if="v$.form.pais.$dirty">
        <small class="text-danger" v-if="v$.form.pais.required.$invalid"
          >Pais es requerido</small
        >
      </div>
    </div>
    <h4>Selecciona tus cursos</h4>
    <div class="col col-2 mb-3">
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="javascript"
          id="javascript"
          v-model="v$.form.cursos.$model"
        />
        <label class="form-check-label" for="javascript"> JavaScript </label>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="react"
          id="react"
          v-model="v$.form.cursos.$model"
        />
        <label class="form-check-label" for="react"> React </label>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="node.js"
          id="node.js"
          v-model="v$.form.cursos.$model"
        />
        <label class="form-check-label" for="node.js"> Node.js </label>
      </div>
      <div v-if="v$.form.cursos.$dirty">
        <small class="text-danger" v-if="v$.form.cursos.required.$invalid"
          >Cursos es requerido</small
        >
      </div>
    </div>
    <div class="mb-3">
      <p class="text-danger fw-bold" v-if="errorSubmit">
        Error al enviar el formulario
      </p>
    </div>
    <button type="submit" class="btn btn-primary">Enviar</button>
  </form>
</template>

<script setup>
import { reactive, ref, computed, defineEmits } from "vue";
import useVuelidate from "@vuelidate/core";
import {
  required,
  alpha,
  minLength,
  integer,
  between,
  email,
} from "@vuelidate/validators";

const emit = defineEmits(['submitForm'])

const listaPaises = reactive(["Argentina", "Uruguay", "Chile", "Brazil", "Otro"]);

const errorSubmit = ref(false);

const state = reactive({
  form: {
    nombre: "",
    apellido: "",
    edad: "",
    email: "",
    pais: "",
    cursos: [],
  },
});

const rules = computed(() => ({
  form: {
    nombre: {
      required,
      alpha,
      minLength: minLength(2),
    },
    apellido: {
      required,
      alpha,
      minLength: minLength(2),
    },
    edad: {
      required,
      integer,
      between: between(18, 100),
    },
    email: {
      required,
      email,
    },
    pais: {
      required,
    },
    cursos: {
      required,
    },
  },
}));

const v$ = useVuelidate(rules, state);

const capitalize = (value) => value.replace(/\b\w/g, (l) => l.toUpperCase());

const resetForm = () => {
    Object.keys(state.form).forEach((key) => (state.form[key] = ""));
    state.form.cursos = [];
}

const submitForm = () => {
    v$.value.$touch()
    if(v$.value.$invalid) {
        errorSubmit.value = true
    } else {
        errorSubmit.value = false
        emit('submitForm', {
            nombre: capitalize(state.form.nombre),
            apellido: capitalize(state.form.apellido),
            edad: state.form.edad,
            email: state.form.email.toLowerCase(),
            pais: state.form.pais,
            cursos: state.form.cursos,
        })
        v$.value.$reset()
        resetForm()
    }
}

// submitForm() {
//     this.$v.$touch()
//     if(this.$v.$invalid) {
//     this.errorSubmit = true
//     } else {
//     this.errorSubmit = false
//     this.$emit("submit-form", {
//         nombre: this.capitalize(this.form.nombre),
//         apellido: this.capitalize(this.form.apellido),
//         edad: this.form.edad,
//         email: this.form.email.toLowerCase(),
//         pais: this.form.pais,
//         cursos: this.form.cursos,
//     });
//     this.$v.$reset()
//     this.resetForm()
//     }
// };
</script>

<style>
</style>