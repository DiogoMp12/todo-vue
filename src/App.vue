<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    { titulo: 'Estudar ES6', finalizada: false },
    { titulo: 'Estudar SASS', finalizada: false },
    { titulo: 'Ir para a academia', finalizada: true },
  ],
});

const getTarefasPendentes = () => estado.tarefas.filter(tarefa => !tarefa.finalizada);

const getTarefasFiltradas = () => {
  const { filtro } = estado;
  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return estado.tarefas.filter(tarefa => tarefa.finalizada);
    default:
      return estado.tarefas;
  }
};

const cadastraTarefa = () => {
  if (estado.tarefaTemp.trim()) {
    estado.tarefas.push({ titulo: estado.tarefaTemp, finalizada: false });
    estado.tarefaTemp = '';
  }
};
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario
      :trocar-filtro="evento => estado.filtro = evento.target.value"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>