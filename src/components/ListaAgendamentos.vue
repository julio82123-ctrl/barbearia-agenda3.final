<script setup>
import { computed } from 'vue'

const props = defineProps({
  agendamentos: { type: Array, required: true },
})
defineEmits(['editar', 'excluir'])

// Ordena por data e depois por horário
const ordenados = computed(() =>
  [...props.agendamentos].sort((a, b) =>
    (a.data + a.horario).localeCompare(b.data + b.horario)
  )
)

// Transforma 2026-11-23 em 23/11/2026
function formatarData(data) {
  const [ano, mes, dia] = data.split('-')
  return `${dia}/${mes}/${ano}`
}
</script>

<template>
  <section class="card">
    <h2>Agendamentos ({{ agendamentos.length }})</h2>

    <p v-if="agendamentos.length === 0" class="aviso">
      Nenhum agendamento ainda. Clique em "Novo agendamento" para criar o primeiro.
    </p>

    <ul v-else class="lista">
      <li v-for="a in ordenados" :key="a.id" class="item">
        <div class="quando">
          <strong>{{ a.horario }}</strong>
          <span>{{ formatarData(a.data) }}</span>
        </div>

        <div class="info">
          <strong>{{ a.cliente }}</strong>
          <span>Barbeiro: {{ a.barbeiro }}</span>
        </div>

        <div class="acoes">
          <button class="btn btn-pequeno btn-cinza" @click="$emit('editar', a)">
            Editar
          </button>
          <button class="btn btn-pequeno btn-perigo" @click="$emit('excluir', a.id)">
            Excluir
          </button>
        </div>
      </li>
    </ul>
  </section>
</template>