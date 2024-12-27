<template>
  <div class=" space-y-4 py-6 px-2.5">
    <form @submit.prevent="handleSubmit" class="space-y-4">
      <h1 class="text-2xl font-bold py-4 text-neutral-600">
          Dados Básicos 
        </h1>
      <div>
        <label for="name">
          <User />
          Nome Completo
        </label>
        <input v-model="formData.name" type="text" id="name" placeholder="Seu nome completo" />
      </div>

      <div>
        <label for="email">
          <Mail />E-mail
        </label>
        <input v-model="formData.email" type="email" id="email" placeholder="Seu melhor e-mail" />
      </div>

      <div>
        <label>
          <Tag />
          Setor de Atuação
        </label>
        <select v-model="formData.sector" name="HeadlineAct" id="HeadlineAct">
          <option value="">Selecione o setor</option>
          <option value="Tecnologia">Tecnologia</option>
          <option value="Servicos">Serviços</option>
          <option value="Varejo">Varejo</option>
          <option value="Industria">Indústria</option>
          <option value="Outro">Outro</option>
        </select>
      </div>

      <div class="space-x-16 flex py-8">
        <Button variant="secondary" type="button" @click="emit('previous')">
          Voltar
        </Button>
        <Button type="submit" :disabled="isSubmitting">
          {{ isSubmitting ? 'Enviando...' : 'Próximo' }}
          <MoveRight :size="25" />
        </Button>
      </div>
    </form>
  </div>
</template>

<style>
input,
select {
  @apply mt-2 block w-full rounded-md border-zinc-300 border-[1px] p-2.5 shadow-sm focus:outline-none text-gray-500 focus:border-zinc-400 focus:ring-1 focus:shadow-md focus:ring-zinc-400;
}

label {
  @apply text-sm font-medium text-gray-700 flex items-center justify-normal gap-1.5;
}
</style>

<script setup lang="ts">
import { User, Mail, Tag, MoveRight } from 'lucide-vue-next';
import Button from '@/components/Buttons/Button.vue';
interface FormData {
  name: string
  email: string
  sector: string
}

const emit = defineEmits(['submit', 'previous'])
const formData = ref<FormData>({
  name: '',
  email: '',
  sector: ''
})
const isSubmitting = ref(false)

const handleSubmit = () => {
  emit('submit', formData.value)
}
</script>