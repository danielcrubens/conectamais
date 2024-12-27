<template>
  <div class=" flex items-center justify-center">
    <div class=" shadow-xl overflow-hidden">
      <div class=" p-8 text-neutral-600 text-center">
        <h1 class="text-2xl font-bold mb-4">
          Mapeamento de Necessidades
        </h1>
        <p class=" text-lg mb-10">
          Entenda suas necessidades digitais e receba soluções personalizadas para impulsionar seu negócio
        </p>
        <Button variant="primary" @click="emit('next')">
          Começar Jornada
          <MoveRight :size="25" />
        </Button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { MoveRight } from 'lucide-vue-next';
import Button from '@/components/Buttons/Button.vue';
const emit = defineEmits(['next'])
</script>