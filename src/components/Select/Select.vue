<script setup>
import { computed } from 'vue'

const emit = defineEmits(['update:modelValue'])

const props = defineProps({
  modelValue: [Number, String],
  label: String,
  helper: String,
  required: Boolean,
  disabled: Boolean,
  readonly: Boolean
})

const rootClasses = computed(() => {
  return {
    'mv-select': true,
    'mv-select--filled': props.modelValue
  }
})

const inputClasses = computed(() => {
  return {
    'mv-select__input': true,
    'mv-select__input--readonly': props.readonly
  }
})

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
  <div :class="rootClasses">
    <select
      v-model="computedValue"
      :class="inputClasses"
      :required="props.required"
      :disabled="props.readonly || props.disabled"
    >
      <option :value="null" v-if="!props.required"></option>
      <slot/>
    </select>
    <label>
      {{ props.label }}
    </label>
    <small v-if="props.helper">
      {{ props.helper }}
    </small>
    <div class="select__outline"></div>
  </div>
</template>

<style lang="scss">
.mv-select {
  margin: 0.5rem 0 1.5rem 0;
  position: relative;

  label {
    color: var(--muted-color);
    padding: 0 0.25rem;
    background: #fff;
    position: absolute;
    top: 1rem;
    left: 0.75rem;
    z-index: 2;
    transition: top 0.15s, font-size 0.15s, color 0.15s;
    pointer-events: none;
  }

  &__input {
    color: transparent;
    padding: 0 3rem 0 1rem;
    background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" height="24" width="24" fill="%235f6368"><path d="m12 15-5-5h10Z"/></svg>') no-repeat calc(100% - 0.75rem) center;
    width: 100%;
    height: 3.5rem;

    &:focus {
      color: var(--dark-text-color);
      outline: none;
    }

    &--readonly {
      cursor: not-allowed;
    }
  }

  small {
    color: var(--muted-color);
    font-size: 0.75rem;
    position: absolute;
    left: 1rem;
    bottom: -1.25rem;
  }

  &__outline {
    display: block;
    border: 1px solid var(--border-color);
    border-radius: 0.25rem;
    height: 3.5rem;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    transition: border-color 0.15s;
    pointer-events: none;
  }

  &--filled {
    .mv-select__input {
      color: var(--dark-text-color);
    }

    label {
      font-size: 0.75rem;
      top: -0.5rem;
    }
  }

  &__input:hover:not(:disabled, :focus) {
    ~ label {
      color: var(--dark-text-color);
    }

    ~ .mv-select__outline {
      border-color: var(--dark-text-color);
    }
  }

  &__input:focus {
    ~ label {
      color: var(--primary-color);
      font-size: 0.75rem;
      top: -0.5rem;
    }

    ~ .mv-select__outline {
      border: 2px solid var(--primary-color);
    }
  }

  &__input:disabled:not(.mv-select__input--readonly) {
    opacity: 0.38;
    cursor: not-allowed;

    ~ label,
    ~ .mv-select__outline {
      opacity: 0.38;
    }
  }
}
</style>