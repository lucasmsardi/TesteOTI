<template>
  <div class="full-screen-box">
    <header class="header">
      <h1>Localizador</h1>
    </header>

    <div class="form-box">
      <Home />
    </div>

    <footer class="footer">
      <p>© {{ currentYear }} Teste Técnico OTI Software</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import Home from "./Home.vue";
const currentYear: Number = new Date().getFullYear();
</script>

<style scoped>
.full-screen-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
}

.header,
.footer {
  width: 100%;
  background-color: #4f46e5;
  color: white;
  padding: 10px;
  text-align: center;
}

.form-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  flex: 1;
  padding: 20px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  min-width: 60vw;
  max-width: 1200px;
}
</style>
