<script setup>
import { ref } from 'vue'
import { USUARIO, SENHA } from '../dados'
import Logo from './Logo.vue'

const emit = defineEmits(['entrar'])

const usuario = ref('')
const senha = ref('')
const erro = ref('')

function entrar() {
  if (usuario.value === USUARIO && senha.value === SENHA) {
    erro.value = ''
    emit('entrar')
  } else {
    erro.value = 'Usuário ou senha incorretos.'
  }
}
</script>

<template>
  <div class="login-tela">
    <form class="card login" @submit.prevent="entrar">
      <div class="login-logo">
        <Logo :tamanho="72" />
        <h1>Barbearia</h1>
        <p class="sub">Entre para acessar a agenda</p>
      </div>

      <label>
        Usuário
        <input v-model="usuario" type="text" autocomplete="username" autofocus />
      </label>

      <label>
        Senha
        <input v-model="senha" type="password" autocomplete="current-password" />
      </label>

      <p v-if="erro" class="erro">{{ erro }}</p>

      <button class="btn btn-largo" type="submit">Entrar</button>
    </form>
  </div>
</template>