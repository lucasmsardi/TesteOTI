<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const estado = ref("");
const cidade = ref("");
const rua = ref("");
const enderecos = ref<Endereco[]>([]);
const erro = ref("");

async function procurarPorRua() {
  erro.value = "";
  enderecos.value = [];

  try {
    const response = await axios.get(
      `https://viacep.com.br/ws/${estado.value}/${cidade.value}/${rua.value}/json/`
    );

    if (response.data.length === 0) {
      erro.value = "Endereço não encontrado!";
    } else {
      enderecos.value = response.data as Endereco[];
    }
  } catch (error: any) {
    if (error.response) {
      switch (error.response.status) {
        case 400:
          erro.value = "Requisição inválida. Verifique os dados informados.";
          break;
        case 404:
          erro.value = "Endereço não encontrado!";
          break;
        case 500:
          erro.value = "Erro no servidor. Tente novamente mais tarde.";
          break;
        default:
          erro.value = "Erro desconhecido. Tente novamente mais tarde.";
          break;
      }
    } else if (error.request) {
      erro.value = "Sem resposta do servidor. Tente novamente mais tarde.";
    } else {
      erro.value =
        "Erro ao configurar a requisição. Tente novamente mais tarde.";
    }
    console.error(error);
  }
}

function limpar() {
  estado.value = "";
  cidade.value = "";
  rua.value = "";
  enderecos.value = [];
  erro.value = "";
}
</script>

<template>
  <div class="form-container">
    <h2>Procurando por Rua:</h2>

    <input
      v-model="estado"
      type="text"
      placeholder="Digite o Estado (UF)"
      class="form-input"
      maxlength="2"
    />

    <input
      v-model="cidade"
      type="text"
      placeholder="Digite a Cidade"
      class="form-input"
    />

    <input
      v-model="rua"
      type="text"
      placeholder="Digite o Nome da Rua"
      class="form-input"
    />

    <div class="button-container">
      <button @click="procurarPorRua" class="form-button">Procurar</button>
      <button @click="limpar" class="form-button form-button-clear">
        Limpar
      </button>
    </div>

    <p v-if="erro" class="error-message">{{ erro }}</p>

    <div class="response" v-if="enderecos.length > 0">
      <h3>Endereços Encontrados:</h3>
      <div
        v-for="(endereco, index) in enderecos"
        :key="index"
        class="endereco-item response"
      >
        <p>
          <strong>Endereço {{ index + 1 }}:</strong>
        </p>
        <p><strong>CEP:</strong> {{ endereco.cep }}</p>
        <p><strong>Rua:</strong> {{ endereco.logradouro }}</p>
        <p><strong>Bairro:</strong> {{ endereco.bairro }}</p>
        <p><strong>Cidade:</strong> {{ endereco.localidade }}</p>
        <p><strong>UF:</strong> {{ endereco.uf }}</p>
        <div
          class="separator"
          v-if="enderecos.length > 1 && index < enderecos.length - 1"
        ></div>
      </div>
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
}

.form-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  gap: 10px;
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

.separator {
  min-width: 300px;
  border: 1px solid;
}
</style>
