<template>
  <footer>
    <p>© {{ currentYear }} Teste Técnico OTI Software</p>
  </footer>
</template>

<script setup lang="ts">
const currentYear: Number = new Date().getFullYear();
</script>

<style scoped>
footer {
  background: #f0f0f0;
  padding: 10px;
  text-align: center;
}
</style>
