<script setup>
import { computed, provide } from 'vue'
import RadioButton from '../RadioButton/RadioButton.vue'

const emit = defineEmits(['update:modelValue'])

const props = defineProps({
  modelValue: [Number, String, Boolean, Array],
  label: String,
  options: Array,
  multiple: Boolean,
  required: Boolean,
  disabled: Boolean
})

const computedValue = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})

provide('selection-name', Math.random())
provide('selection-required', props.required)
</script>

<template>
  <fieldset class="mv-selection">
    <legend v-if="label">{{ label }}</legend>
    <div class="mv-selection-options">
      <radio-button
        v-for="option in props.options"
        :key="option.value || option"
        :label="option.label || option.value || option"
        :helper="option.helper"
        :value="option.value || option"
        :disabled="props.disabled"
        v-model="computedValue"
      />
      <slot/>
    </div>
  </fieldset>
</template>

<style lang="scss">
.mv-selection {
  margin: 0.5rem 0 1.5rem;

  legend {
    color: var(--muted-color);
    font-size: 0.75rem;
    margin-bottom: 1rem;
  }

  &-options {
    display: grid;
    gap: 1.25rem;
  }
}
</style>