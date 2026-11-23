<script setup>
import { ref, reactive, computed, watch } from 'vue'
import { BARBEIROS, HORARIOS } from '../dados'

const props = defineProps({
  agendamentos: { type: Array, required: true },
  editando: { type: Object, default: null },
})
const emit = defineEmits(['salvar', 'cancelar'])

const erro = ref('')

function formVazio() {
  return { id: null, cliente: '', barbeiro: '', data: '', horario: '' }
}
const form = reactive(formVazio())

// Data de hoje no formato do input date (AAAA-MM-DD), sem bug de fuso horário
const hoje = new Date().toLocaleDateString('en-CA')

// Quando clicar em "Editar" na lista, preenche o formulário com os dados
watch(
  () => props.editando,
  (novo) => {
    Object.assign(form, novo ? { ...novo } : formVazio())
    erro.value = ''
  }
)

// Se trocar a data ou o barbeiro, o horário escolhido pode já estar ocupado
watch(
  () => [form.data, form.barbeiro],
  () => {
    if (form.horario && !horarioLivre(form.horario)) form.horario = ''
  }
)

// Um horário está livre se ninguém marcou com o mesmo barbeiro na mesma data
function horarioLivre(horario) {
  return !props.agendamentos.some(
    (a) =>
      a.data === form.data &&
      a.barbeiro === form.barbeiro &&
      a.horario === horario &&
      a.id !== form.id
  )
}

const horarios = computed(() =>
  HORARIOS.map((h) => ({ hora: h, livre: horarioLivre(h) }))
)

const podeEscolherHorario = computed(() => form.data && form.barbeiro)

function enviar() {
  if (!form.cliente.trim() || !form.barbeiro || !form.data || !form.horario) {
    erro.value = 'Preencha todos os campos e escolha um horário.'
    return
  }
  erro.value = ''
  emit('salvar', { ...form, cliente: form.cliente.trim() })
  Object.assign(form, formVazio())
}

function cancelar() {
  Object.assign(form, formVazio())
  erro.value = ''
  emit('cancelar')
}
</script>

<template>
  <form class="card" @submit.prevent="enviar">
    <h2>{{ form.id ? 'Editar agendamento' : 'Novo agendamento' }}</h2>

    <div class="linha">
      <label>
        Nome do cliente
        <input v-model="form.cliente" type="text" placeholder="Ex.: João Silva" />
      </label>

      <label>
        Barbeiro
        <select v-model="form.barbeiro">
          <option value="" disabled>Escolha o barbeiro</option>
          <option v-for="b in BARBEIROS" :key="b" :value="b">{{ b }}</option>
        </select>
      </label>

      <label>
        Data
        <input v-model="form.data" type="date" :min="hoje" />
      </label>
    </div>

    <div>
      <p class="rotulo">Horário</p>

      <p v-if="!podeEscolherHorario" class="aviso">
        Escolha o barbeiro e a data para ver os horários disponíveis.
      </p>

      <div v-else class="horarios">
        <button
          v-for="h in horarios"
          :key="h.hora"
          type="button"
          class="horario"
          :class="{ ativo: form.horario === h.hora }"
          :disabled="!h.livre"
          :title="h.livre ? 'Disponível' : 'Já está ocupado'"
          @click="form.horario = h.hora"
        >
          {{ h.hora }}
        </button>
      </div>
    </div>

    <p v-if="erro" class="erro">{{ erro }}</p>

    <div class="acoes">
      <button class="btn" type="submit">
        {{ form.id ? 'Salvar alterações' : 'Agendar' }}
      </button>
      <button v-if="form.id" class="btn btn-cinza" type="button" @click="cancelar">
        Cancelar
      </button>
    </div>
  </form>
</template>