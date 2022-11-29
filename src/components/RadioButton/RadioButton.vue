<script setup>
import { computed, inject } from 'vue'

const emit = defineEmits(['update:modelValue'])

const props = defineProps({
  modelValue: [Boolean, Number, String],
  value: [Boolean, Number, String],
  label: [Number, String],
  helper: String,
  disabled: Boolean
})

const id = Math.random()

const selection = {
  name: inject('selection-name', null),
  required: inject('selection-required', null)
}

const computedValue = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})
</script>

<template>
  <div class="mv-radio-button">
    <input
      v-model="computedValue"
      class="mv-radio-button__input"
      type="radio"
      :id="id"
      :name="selection.name"
      :value="props.value"
      :required="selection.required"
      :disabled="props.disabled"
    >
    <div class="mv-radio-button__state"></div>
    <label :for="id" class="mv-radio-button__label">
      <div class="mv-radio-button__label__text">
        {{ props.label }}
      </div>
      <div class="mv-radio-button__helper" v-if="props.helper">
        {{ props.helper }}
      </div>
    </label>
  </div>
</template>

<style lang="scss">
.mv-radio-button {
  display: flex;
  gap: 1.125rem;
  width: fit-content;
  position: relative;

  &__input {
    flex-shrink: 0;
    border: 2px solid var(--muted-color);
    border-radius: 50%;
    width: 1.25rem;
    height: 1.25rem;
    z-index: 3;
    transition: border 0.15s, background-color 0.15s;
    appearance: none !important;
    cursor: pointer;

    &:not(:checked):not(:disabled) {
      &:focus,
      &:hover {
        border-color: var(--dark-text-color);

        ~ .mv-radio-button__state {
          background-color: rgb(60 64 67 / 5%);
        }
      }
    }

    &:checked {
      border-color: var(--primary-color);
      outline: 3.5px solid var(--background-color);
      outline-offset: -5px;
      background-color: var(--primary-color);

      &:hover {
        border-color: var(--dark-color);
        outline-color: var(--light-color);
        background-color: var(--dark-color);

        ~ .mv-radio-button__state {
          background-color: var(--light-color);
        }
      }
    }

    &:disabled {
      opacity: 0.38;
      cursor: default;

      ~ .mv-radio-button__state {
        background-color: transparent;
      }

      ~ label {
        opacity: 0.38;
      }
    }
  }

  &__state {
    display: grid;
    place-content: center;
    border-radius: 50%;
    width: 2.5rem;
    height: 2.5rem;
    position: absolute;
    top: -0.625rem;
    left: -0.625rem;
    z-index: 1;
    transition: background-color 0.15s;
  }

  &__label {
    cursor: default !important;

    &__text {
      display: flex;
      align-items: center;
      color: var(--dark-text-color);
      font-size: 0.875rem;
      line-height: 1.25rem;
      min-height: 1.25rem;
    }
  }

  &__helper {
    color: var(--muted-color);
    font-size: 0.75rem;
    margin-top: 0.125rem;
  }
}
</style>