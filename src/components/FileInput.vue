<template>
  <div>
    <button class="file-input" @click="triggerFileInput">Cargar</button>
    <input ref="fileInput" hidden type="file" @change="handleFileSelect" />
    <p class="description">{{ message }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import axios from 'axios'

const file = ref<File | null>(null)
const isLoading = ref<boolean>(false)
const hasError = ref<boolean>(false)
const fileInput = ref<HTMLInputElement | null>(null)

const message = computed(() => {
  if (hasError.value && !isLoading.value) {
    return 'Error, intenta luego'
  }

  if (!hasError.value && isLoading.value) {
    return 'Cargando...'
  }

  return 'Carga tus documentos aqui'
})

const webhookUrl = import.meta.env.VITE_WEBHOOK_URL as string

function triggerFileInput(): void {
  fileInput.value?.click()
}

async function handleFileSelect(e: Event): Promise<void> {
  isLoading.value = true
  hasError.value = false

  const input = e.target as HTMLInputElement
  const filesAsArray = Array.from(input?.files || [])
  file.value = filesAsArray[0]

  const formData = new FormData()
  formData.append('document', file.value)
  formData.append('filename', file.value.name)
  formData.append('mimetype', file.value.type)

  try {
    await axios.post(`${webhookUrl}/upload-file`, formData)
    alert('Documento cargado exitosamente')
  } catch (err) {
    hasError.value = true
    setTimeout(() => {
      hasError.value = false
    }, 1500)
    console.error(err)
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.file-input {
  background-color: rgb(60, 60, 227);
  color: #fff;
  padding: 1rem 1.8rem;
  border-radius: 8px;
  display: block;
  border: none;
  margin-top: 1.2rem;
  margin: 1.2rem auto 0 auto;
}

.file-input:hover {
  cursor: pointer;
}

.description {
  margin-top: 1.2rem;
}
</style>
