<template>
  <main class="columns is-gapless is-mutiline" :class="{ 'modo-escuro': modoEscuroAtivo }">
    <div class="column is-one-quarter">
      <BarraLateral @aoTemaAlterado="trocarTema"/>
    </div>
    <div class="column is-three-quarter conteudo">
      <Formulario @aoSalvarTarefa="salvarTarefa" />
      <div class="lista">
        
        <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa" />
        <Box v-if="listaEstaVazia">
          Você não está muito produtivo hoje :)
        </Box>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import BarraLateral from "./components/BarraLAteral.vue";
import Box from "./components/Box.vue";
import Formulario from "./components/Formulario.vue";
import Tarefa from "./components/Tarefa.vue";
import ITarefa from "./interfaces/ITarefa";

export default defineComponent({
  name: "App",
  components: {
    BarraLateral,
    Formulario,
    Tarefa,
    Box,
  },
  data() {
    return{
      tarefas: [] as ITarefa[],
      modoEscuroAtivo: false
    }
  },
  methods: {
    salvarTarefa(tarefa: ITarefa){
      this.tarefas.push(tarefa)
    },
    trocarTema(modoEscuroAtivo: boolean){
      this.modoEscuroAtivo = modoEscuroAtivo
    }

  },
  computed: {
    listaEstaVazia() : boolean{
      return this.tarefas.length === 0
    }
  }
});
</script>

<style>
.lista {
  padding: 1.25rem;
}
main{
  --bg-primario: #fff;
  --texto-primario: #000;

}
main.modo-escuro{
  --bg-primario: #2b2d42;
  --texto-primario: #ddd;
}
.conteudo{
  background: var(--bg-primario);
}
</style>
