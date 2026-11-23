<script setup>
import Logo from './Logo.vue'

defineProps({
  tela: { type: String, required: true },
})
defineEmits(['ir', 'sair'])

const itens = [
  { nome: 'agendamentos', texto: 'Agendamentos' },
  { nome: 'novo', texto: 'Novo agendamento' },
]
</script>

<template>
  <aside class="sidebar">
    <div class="marca">
      <Logo :tamanho="40" />
      <div class="marca-textos">
        <span class="marca-nome">BARBEARIA</span>
        <span class="marca-sub">Agenda online</span>
      </div>
    </div>

    <nav class="menu">
      <button
        v-for="item in itens"
        :key="item.nome"
        class="menu-item"
        :class="{ ativo: tela === item.nome }"
        @click="$emit('ir', item.nome)"
      >
        {{ item.texto }}
      </button>
    </nav>

    <button class="menu-item" @click="$emit('sair')">Sair</button>
  </aside>
</template>