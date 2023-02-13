<template>
  <section class="projetos">
    <h1>Projetos</h1>
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
import {  defineComponent } from "vue";
import { useStore } from "@/store";

export default defineComponent({
  name: "Formulario",
  props: {
    id: {
      type: String
    }
  },
  mounted(){
    if(this.id){
      const projeto = this.store.state.projetos.find(proj => proj.id == this.id)
      this.nomeDoProjeto = projeto?.nome || ''
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
      if(this.id){
        this.store.commit('ALTERA_PROJETO', {
          id: this.id,
          nome: this.nomeDoProjeto
        })

      }else{
        this.store.commit('ADICIONA_PROJETO', this.nomeDoProjeto)
      }
      
      this.nomeDoProjeto = "";
      this.$router.push('/projetos')
    },
  },
  setup(){
    const store = useStore()
    return {
      store
    }
  }
});
</script>

<style scoped>
.projetos {
  padding: 1.25rem;
}
</style>