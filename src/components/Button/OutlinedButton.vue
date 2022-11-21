<script setup>
import { computed } from 'vue'
import commonProps from './commonProps'

const props = defineProps(commonProps)

const rootClasses = computed(() => {
  return {
    'mv-button': true,
    'mv-button--large': props.large,
    'mv-button--wide': props.wide,
    'mv-button--loading': props.loading,
    'mv-button--disabled': props.disabled,
    'mv-outlined-button': true
  }
})
</script>

<template>
  <button :class="rootClasses" :type="props.type" :disabled="props.disabled" v-if="to">
    <slot></slot>
  </button>

  <router-link :class="rootClasses" :to="to" :inert="props.disabled" v-else>
    <slot></slot>
  </router-link>
</template>

<style lang="scss">
@import './commonStyles.scss';

.mv-outlined-button {
  outline: 1px solid var(--border-color);
  outline-offset: -1px;
  --color: var(--primary-color);
  --background-color: none;
  --spinner-color: var(--primary-color);

  &:hover:not(:disabled) {
    --background-color: var(--light-color);
  }

  &:disabled {
    opacity: 0.38;
    --color: var(--text-color);
  }
}
</style>