<template>
  <button type="button" :class="{
    'flex justify-center items-center gap-3 w-full text-lg font-semibold py-2.5 rounded-md': true,
    ...VariantClass,
  }" v-bind="$attrs">
    <slot />
  </button>
</template>

<script setup>
import { computed, defineProps } from 'vue';

const props = defineProps({
  variant: {
    type: String,
    default: 'primary',
    validator(value) {
      return ['primary', 'secondary'].includes(value);
    },
  },
 
});

const VariantClass = computed(() => ({
  'bg-blue text-white hover:brightness-90': props.variant === 'primary',
  'bg-white text-blue': props.variant === 'secondary',
}));


</script>