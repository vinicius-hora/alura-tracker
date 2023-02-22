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
import { defineComponent, ref } from "vue";
import { useStore } from "@/store";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import useNotificador from "@/hooks/notificador";
import { ALTERAR_PROJETO, CADASTRAR_PROJETO } from "@/store/tipo-acoes";
import { useRouter } from "vue-router";

export default defineComponent({
  name: "Formulario",
  props: {
    id: {
      type: String,
    },
  },
  methods: {
  
   
  },
  setup(props) {
    const router = useRouter();

    const store = useStore();
    const { notificar } = useNotificador();

    const nomeDoProjeto = ref("")

    if (props.id) {
      const projeto = store.state.projeto.projetos.find(
        (proj) => proj.id == props.id
      );
      nomeDoProjeto.value = projeto?.nome || "";
    }
    const sucesso =() => {
      nomeDoProjeto.value = "";
      router.push("/projetos");
      notificar(
        TipoNotificacao.SUCESSO,
        "Excelente",
        "projeto cadastrado"
      );
    }

    const salvar = () => {
      if (props.id) {
        store
          .dispatch(ALTERAR_PROJETO, {
            id: props.id,
            nome: nomeDoProjeto.value,
          })
          .then(() => sucesso());
      } else {
        store
          .dispatch(CADASTRAR_PROJETO, nomeDoProjeto.value)
          .then(() => sucesso());
      }
    }
    
    return {
      nomeDoProjeto,
      salvar,
    };
  },
});
</script>

<style scoped>
</style>