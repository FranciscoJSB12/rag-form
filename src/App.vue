<template>
  <div class="upload-form">
    <h1>Formulario de carga</h1>
    <FileInput />
  </div>
</template>

<script setup lang="ts">
import FileInput from './components/FileInput.vue'
</script>

<style scoped>
.upload-form {
  width: 100%;
  max-width: 450px;
  border: 2px solid #ddd;
  padding: 3rem 1.2rem;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 8rem auto 0 auto;
}
</style>
