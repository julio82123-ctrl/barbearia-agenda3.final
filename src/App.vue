<script setup>
import { ref, watch } from 'vue'
import Login from './components/Login.vue'
import Sidebar from './components/Sidebar.vue'
import FormAgendamento from './components/FormAgendamento.vue'
import ListaAgendamentos from './components/ListaAgendamentos.vue'

const logado = ref(localStorage.getItem('logado') === 'true')
const agendamentos = ref(JSON.parse(localStorage.getItem('agendamentos') || '[]'))
const editando = ref(null)

// Qual "sala" está aberta: 'agendamentos' ou 'novo'
const tela = ref('agendamentos')

watch(
  agendamentos,
  (valor) => localStorage.setItem('agendamentos', JSON.stringify(valor)),
  { deep: true }
)

function entrar() {
  logado.value = true
  localStorage.setItem('logado', 'true')
  tela.value = 'agendamentos'
}

function sair() {
  logado.value = false
  editando.value = null
  localStorage.removeItem('logado')
}

function irPara(nome) {
  editando.value = null
  tela.value = nome
}

function salvar(agendamento) {
  if (agendamento.id) {
    // Já existe: atualiza (EDITAR)
    const posicao = agendamentos.value.findIndex((a) => a.id === agendamento.id)
    if (posicao !== -1) agendamentos.value[posicao] = agendamento
  } else {
    // Não existe: cria um novo (ADICIONAR)
    agendamentos.value.push({ ...agendamento, id: Date.now() })
  }
  editando.value = null
  tela.value = 'agendamentos'
}

function excluir(id) {
  agendamentos.value = agendamentos.value.filter((a) => a.id !== id)
}

function editar(agendamento) {
  editando.value = { ...agendamento }
  tela.value = 'novo'
}

function cancelar() {
  editando.value = null
  tela.value = 'agendamentos'
}
</script>

<template>
  <Login v-if="!logado" @entrar="entrar" />

  <div v-else class="app">
    <Sidebar :tela="tela" @ir="irPara" @sair="sair" />

    <main class="conteudo">
      <div class="coluna">
        <ListaAgendamentos
          v-if="tela === 'agendamentos'"
          :agendamentos="agendamentos"
          @editar="editar"
          @excluir="excluir"
        />

        <FormAgendamento
          v-else
          :key="editando ? editando.id : 'novo'"
          :agendamentos="agendamentos"
          :editando="editando"
          @salvar="salvar"
          @cancelar="cancelar"
        />
      </div>
    </main>
  </div>
</template>