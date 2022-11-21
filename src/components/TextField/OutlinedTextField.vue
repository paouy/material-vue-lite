<script setup>
import { computed } from 'vue'

const emit = defineEmits(['update:modelValue', 'blur', 'focus'])

const props = defineProps({
  modelValue: [Number, String],
  label: String,
  type: {
    type: String,
    default: 'text'
  },
  placeholder: [Number, String],
  minlength: [Number, String],
  maxlength: [Number, String],
  helper: String,
  prefix: String,
  suffix: String,
  required: Boolean,
  disabled: Boolean,
  readonly: Boolean
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
  <div class="mv-outlined-text-field">
    <input
      v-model="computedValue"
      class="mv-outlined-text-field__input"
      :type="type"
      :placeholder="props.placeholder || ' '"
      :minlength="props.minlength"
      :maxlength="props.maxlength"
      :disabled="props.disabled"
      :readonly="props.readonly"
      :required="props.required"
      @blur="emit('blur')"
      @focus="emit('focus')"
    >
    <div class="mv-outlined-text-field__prefix" v-if="props.prefix">
      {{ props.prefix }}
    </div>

    <div class="mv-outlined-text-field__suffix" v-if="props.suffix">
      {{ props.suffix }}
    </div>

    <label>{{ props.label }}</label>

    <small v-if="props.helper">{{ props.helper }}</small>

    <div class="mv-outlined-text-field__outline"></div>
  </div>
</template>

<style lang="scss">
.mv-outlined-text-field {
  display: flex;
  align-items: flex-start;
  gap: 0.5rem;
  margin: 0.5rem 0 1.5rem 0;
  padding: 0 1rem;
  width: 100%;
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
    order: 2;
    color: var(--dark-mv-outlined-text-color);
    flex: 1 0 0;
    height: 3.5rem;

    &:focus {
      outline: none;
    }
  }

  &__prefix {
    order: 1;
    display: none;
  }

  &__suffix {
    order: 3;
    display: none;
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

  &__input:not(:placeholder-shown),
  &__input:focus {
    ~ label {
      font-size: 0.75rem;
      top: -0.5rem;
    }

    ~ .mv-outlined-text-field__prefix,
    ~ .mv-outlined-text-field__suffix {
      display: block;
      color: var(--dark-mv-outlined-text-color);
      line-height: 3.5rem;
    }
  }

  &__input:focus:not(:read-only) {
    ~ label {
      color: var(--primary-color);
    }

    ~ .mv-outlined-text-field__outline {
      border: 2px solid var(--primary-color);
    }
  }

  &__input:hover:not(:focus, :disabled, :read-only) {
    ~ label {
      color: var(--dark-mv-outlined-text-color);
    }

    ~ .mv-outlined-text-field__outline {
      border-color: var(--dark-mv-outlined-text-color);
    }
  }

  &__input:disabled {
    opacity: 0.38;
    cursor: not-allowed;

    ~ label,
    ~ .mv-outlined-text-field__outline {
      opacity: 0.38;
    }
  }

  &__input:read-only {
    cursor: not-allowed;
  }
}
</style>