<script setup>
const emit = defineEmits(['update:modelValue', 'change'])

const props = defineProps({
  modelValue: File,
  required: Boolean
})

const onChange = (event) => {
  emit('update:modelValue', event.target.files[0])
  emit('change', event)
}
</script>

<template>
  <div class="mv-file-picker">
    <label class="mv-file-picker__faux-button mv-file-picker__faux-button--input" v-if="!props.modelValue">
      <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20">
        <path d="M5.5 16q-.625 0-1.062-.438Q4 15.125 4 14.5V13h1.5v1.5h9V13H16v1.5q0 .625-.438 1.062Q15.125 16 14.5 16Zm3.75-3V5.875L7.062 8.062 6 7l4-4 4 4-1.062 1.062-2.188-2.187V13Z"/>
      </svg>
      Add file
      <input type="file" @change="onChange" :required="props.required">
    </label>
    <div class="mv-file-picker__faux-button mv-file-picker__faux-button--remove" v-else>
      <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20">
        <path d="M5.5 18q-.625 0-1.062-.438Q4 17.125 4 16.5v-13q0-.625.438-1.062Q4.875 2 5.5 2H12l4 4v10.5q0 .625-.438 1.062Q15.125 18 14.5 18Zm0-1.5h9V7H11V3.5H5.5v13Zm4.25-.5q1.354 0 2.302-.948T13 12.75V10h-1.5v2.75q0 .729-.51 1.24-.511.51-1.24.51t-1.24-.51Q8 13.479 8 12.75V9q0-.208.146-.354T8.5 8.5q.208 0 .354.146T9 9v4h1.5V9q0-.833-.583-1.417Q9.333 7 8.5 7q-.833 0-1.417.583Q6.5 8.167 6.5 9v3.75q0 1.354.948 2.302T9.75 16ZM5.5 3.5v3.938V3.5v13-13Z"/>
      </svg>
      <div class="mv-file-picker__filename">{{ props.modelValue?.name }}</div>
      <button type="button" @click="emit('update:modelValue', null)">Remove</button>
    </div>
  </div>
</template>

<style lang="scss">
.mv-file-picker {
  &__faux-button {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: var(--primary-color);
    font-size: 0.875rem;
    font-weight: 500;
    line-height: 2.5rem;
    border-radius: 1.25rem;
    outline: 1px solid var(--border-color);
    outline-offset: -1px;
    height: 2.5rem;
    width: fit-content;

    svg {
      fill: var(--primary-color);
    }
  }

  &__faux-button--input {
    padding: 0 1.5rem 0 1rem;
    transition: background-color 0.15s, opacity 0.15s;
    position: relative;
    cursor: pointer;

    input {
      opacity: 0;
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      pointer-events: none;
    }

    &:hover:not(:disabled) {
      background-color: var(--light-color);
      opacity: 0.92;
    }
  }

  &__faux-button--remove {
    color: var(--dark-text-color);
    font-weight: normal;
    padding: 0 1rem;

    button {
      font-size: 0;
      background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" height="20" width="20" fill="%235f6368"><path d="M6.062 15 5 13.938 8.938 10 5 6.062 6.062 5 10 8.938 13.938 5 15 6.062 11.062 10 15 13.938 13.938 15 10 11.062Z"/></svg>') no-repeat center;
      width: 1rem;
      height: 1rem;
    }
  }

  &__filename {
    text-overflow: ellipsis;
    white-space: nowrap;
    max-width: 7.5rem;
    overflow: hidden;
  }
}
</style>