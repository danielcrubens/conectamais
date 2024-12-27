<!-- components/steps/ServicesStep.vue -->
<template>
  <div class="space-y-6 py-6 px-2.5">
    <h1 class="text-2xl font-bold py-4 text-neutral-600">
      Serviços de Interesse
    </h1>

    <div class="grid gap-4">
      <div v-for="service in services" :key="service.id"
        class="p-4 border-2 rounded-xl cursor-pointer transition-all duration-200" :class="{
          'border-blue-500 bg-blue-50': selectedServices.includes(service.id),
          'border-zinc-300 hover:border-blue-200': !selectedServices.includes(service.id)
        }" @click="toggleService(service.id)" role="button" :aria-selected="selectedServices.includes(service.id)">
        <div class="flex items-start space-x-4">
          <div class="p-2 rounded-lg" :class="{
            'bg-blue-500 text-white': selectedServices.includes(service.id),
            'bg-gray-100 text-neutral-600': !selectedServices.includes(service.id)
          }">

            <Monitor v-if="service.id === 'sites'" class="w-6 h-6" />
            <Smartphone v-if="service.id === 'webapps'" class="w-6 h-6" />
            <Palette v-if="service.id === 'uxui'" class="w-6 h-6" />
          </div>
          <div>
            <h4 class="font-medium text-gray-800">
              {{ service.title }}
            </h4>
            <p class="text-gray-500 text-sm">
              {{ service.description }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <div class="space-x-2 flex py-8">
      <Button :variant="'secondary'" @click="emit('previous')" :disabled="isSubmitting">
        Voltar
      </Button>
      <Button @click="handleSubmit" :disabled="isSubmitting || selectedServices.length === 0"
        class=" disabled:opacity-50">
        {{ isSubmitting ? 'Enviando...' : 'Finalizar ' }}
      </Button>

    </div>

    <p v-if="errorMessage" class="text-red-500 text-sm text-center mt-2">
      {{ errorMessage }}
    </p>
  </div>
</template>

<script setup lang="ts">
import type { Props, Service } from '~/types'
import { Monitor, Smartphone, Palette } from 'lucide-vue-next';
import Button from '@/components/Buttons/Button.vue';


// Tornando userData obrigatório
const props = defineProps<Props>()
const emit = defineEmits(['update:services', 'next', 'previous', 'completed'])

const { addDocument } = useFirestoreActions()
const selectedServices = ref<string[]>([])
const isSubmitting = ref(false)
const errorMessage = ref('')

const services: Service[] = [
  {
    id: 'sites',
    title: 'Sites Institucionais',
    description: 'Websites profissionais e otimizados para sua empresa'
  },
  {
    id: 'webapps',
    title: 'Aplicativos Web',
    description: 'Sistemas web personalizados para seu negócio'
  },
  {
    id: 'uxui',
    title: 'UX/UI Design',
    description: 'Design de interfaces e experiência do usuário'
  }
]

const toggleService = (serviceId: string) => {
  const index = selectedServices.value.indexOf(serviceId)
  if (index === -1) {
    selectedServices.value.push(serviceId)
  } else {
    selectedServices.value.splice(index, 1)
  }
  emit('update:services', selectedServices.value)
}

const handleSubmit = async () => {
  if (selectedServices.value.length === 0) {
    errorMessage.value = 'Selecione pelo menos um serviço'
    return
  }

  if (!props.userData?.email || !props.userData?.name) {
    errorMessage.value = 'Dados do usuário incompletos'
    return
  }

  isSubmitting.value = true
  errorMessage.value = ''

  try {
    // Garantindo que todos os campos existam antes de salvar
    const servicesData = {
      services: selectedServices.value,
      updatedAt: new Date(),
      userId: props.userData.email, // Email como ID do usuário
      name: props.userData.name,
      email: props.userData.email,
      status: 'pending' // Status inicial do interesse
    }

    const firestoreResult = await addDocument('services-interest', servicesData)

    if (!firestoreResult.success) {
      throw new Error(firestoreResult.error || 'Erro ao salvar os serviços')
    }

    emit('completed', {
      services: selectedServices.value,
      documentId: firestoreResult.id
    })

    selectedServices.value = []
    emit('next')
  } catch (error) {
    console.error('Erro ao salvar serviços:', error)
    errorMessage.value = 'Ocorreu um erro ao salvar os serviços. Tente novamente.'
  } finally {
    isSubmitting.value = false
  }
}
</script>