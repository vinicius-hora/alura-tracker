<template>
  <section>
    <form @submit.prevent="salvar">
      <div class="field">
        <label for="nomeDoProjeto" class="label"> Nome do Projeto </label>
        <input
          type="text"
          name=""
          id="nomeDoProjeto"
          class="input"
          v-model="nomeDoProjeto"
        />
      </div>
      <div class="field">
        <button class="button" type="submit">salvar</button>
      </div>
    </form>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useStore } from "@/store";
import {
  ALTERA_PROJETO,
} from "@/store/tipo-mutacoes";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import useNotificador  from "@/hooks/notificador"
import { ALTERAR_PROJETO, CADASTRAR_PROJETO } from "@/store/tipo-acoes";

export default defineComponent({
  name: "Formulario",
  props: {
    id: {
      type: String,
    },
  },
  mounted() {
    if (this.id) {
      const projeto = this.store.state.projetos.find(
        (proj) => proj.id == this.id
      );
      this.nomeDoProjeto = projeto?.nome || "";
    }
  },
  data() {
    return {
      nomeDoProjeto: "",
    };
  },
  methods: {
    salvar() {
      // const projeto: IProjeto = {
      //   nome: this.nomeDoProjeto,
      //   id: new Date().toISOString(),
      // };
      // this.projetos.push(projeto);
      // this.nomeDoProjeto = "";
      if (this.id) {
        this.store.dispatch(ALTERAR_PROJETO, {
          id: this.id,
          nome: this.nomeDoProjeto,
        }).then(() => this.sucesso());
      } else {
        this.store.dispatch(CADASTRAR_PROJETO, this.nomeDoProjeto)
        .then(() => this.sucesso())
      }

     
    },
    sucesso(){
        this.nomeDoProjeto = "";
          this.$router.push('/projetos');
          this.notificar(TipoNotificacao.SUCESSO, 'Excelente', 'projeto cadastrado')
    }
    
  },
  setup() {
    const store = useStore();
    const { notificar } = useNotificador()
    return {
      store,
      notificar
    };
  },
});
</script>

<style scoped>
</style>