<script setup>
import store from '@/store'
import axios from 'axios'
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import cambioContrasena from './cambioContrasena.vue'

const route = useRoute()
const router = useRouter()

const cedula = route.params.cedula
const token = route.params.token
const mensaje = ref(null)
const email = ref(null)

axios
  .get(`http://localhost:3000/usuarios/puede-recuperar/${cedula}/${token}`)
  .then(response => {
    if (response.data.puede) {
      store.dispatch('cambiar')
      email.value = response.data.email
      //router.push({ path: '/auth' })
    } else {
      mensaje.value = response.data.message
      console.log('Error: ' + response.data.message)
    }
  })
  .catch(error => {
    console.error('Error al verificar:', error)
    mensaje.value = 'Error al verificar la recuperación de contraseña.'
  })
</script>

<template>
  <!-- Renderizar `cambioContrasena` solo si `email` tiene un valor -->
  <cambioContrasena
    v-if="email"
    :email="email"
  />
  <p v-if="mensaje">{{ mensaje }}</p>
</template>
