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
    'mv-filled-button': true
  }
})
</script>

<template>
  <button :class="rootClasses" :type="props.type" :disabled="props.disabled" v-if="!to">
    <slot></slot>
  </button>

  <router-link :class="rootClasses" :to="to" :inert="props.disabled" v-else>
    <slot></slot>
  </router-link>
</template>

<style lang="scss">
@import './commonStyles.scss';

.mv-filled-button {
  --color: #fff;
  --background-color: var(--primary-color);
  --spinner-color: #fff;

  &:hover:not(:disabled) {
    box-shadow: var(--elevation-1);
  }
}
</style>