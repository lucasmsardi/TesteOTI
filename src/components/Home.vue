<template>
  <div class="form-container">
    <nav class="form-nav">
      <button
        @click="formularioSelecionado = 'cep'"
        :class="{ ativo: formularioSelecionado === 'cep' }"
      >
        Procurar por CEP
      </button>
      <button
        @click="formularioSelecionado = 'rua'"
        :class="{ ativo: formularioSelecionado === 'rua' }"
      >
        Procurar por Rua
      </button>
    </nav>

    <div class="form-content">
      <CEPForm v-if="formularioSelecionado === 'cep'" />
      <RuaForm v-else-if="formularioSelecionado === 'rua'" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import CEPForm from "./CEPForm.vue";
import RuaForm from "./RuaForm.vue";

const formularioSelecionado = ref<"cep" | "rua">("cep");
</script>

<style scoped>
.form-container {
  max-width: 400px;
  margin: auto;
  text-align: center;
}

.form-nav {
  margin-bottom: 20px;
}

.form-nav button {
  margin: 0 5px;
  padding: 10px 15px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  border-radius: 4px;
}

.form-nav button.ativo {
  background-color: #4f46e5;
  color: white;
}

.form-content {
  padding: 20px;
  min-height: 300px;
}
</style>
