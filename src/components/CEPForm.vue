<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const cep = ref("");
const endereco = ref<Endereco>();
const erro = ref("");

function formatarCEP(value: string): string {
  if (value.length > 5) {
    return value.replace(/^(\d{5})(\d{1})$/, "$1-$2");
  }

  return value;
}

function limparMascara(value: string): string {
  return value.replace(/\D/g, "");
}

async function procurarPorCEP() {
  erro.value = "";
  endereco.value = {} as Endereco;

  const cepSemMascara = limparMascara(cep.value);

  if (cepSemMascara.length !== 8) {
    erro.value = "O CEP deve conter exatamente 8 dígitos.";
    return;
  }

  try {
    const response = await axios.get(
      `https://viacep.com.br/ws/${cepSemMascara}/json/`
    );

    if (response.data.erro) {
      erro.value = "CEP não encontrado!";
    } else {
      endereco.value = response.data as Endereco;
    }
  } catch (error) {
    erro.value = "Erro ao buscar o CEP. Tente novamente mais tarde.";
    console.error(error);
  }
}

function limpar() {
  cep.value = "";
  endereco.value = undefined;
  erro.value = "";
}

function handleInput(event: Event) {
  const input = event.target as HTMLInputElement;
  cep.value = formatarCEP(input.value);
}
</script>

<template>
  <div class="form-container">
    <h2>Procurando por CEP:</h2>

    <input
      v-model="cep"
      type="text"
      placeholder="Digite o CEP"
      class="form-input"
      @input="handleInput($event)"
    />

    <div class="button-container">
      <button @click="procurarPorCEP" class="form-button">Procurar</button>
      <button @click="limpar" class="form-button form-button-clear">
        Limpar
      </button>
    </div>

    <p v-if="erro" class="error-message">{{ erro }}</p>

    <div class="response" v-if="endereco && !erro">
      <h3>Endereço Encontrado</h3>
      <p><strong>Logradouro:</strong> {{ endereco.logradouro }}</p>
      <p><strong>Bairro:</strong> {{ endereco.bairro }}</p>
      <p><strong>Cidade:</strong> {{ endereco.localidade }}</p>
      <p><strong>UF:</strong> {{ endereco.uf }}</p>
    </div>
  </div>
</template>

<style scoped>
h2 {
  margin-bottom: 10px;
  font-size: 18px;
}

.form-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.button-container {
  display: flex;
  gap: 10px;
}

.form-button {
  width: 100%;
  padding: 10px;
  background-color: #4f46e5;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.form-button-clear {
  background-color: #f44336;
}

.error-message {
  color: red;
  font-size: 14px;
  margin-top: 10px;
}

h3 {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.response {
  gap: 15px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.response p {
  margin: 0px;
}
</style>
