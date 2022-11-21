<script setup>
const props = defineProps({
  to: String,
  disabled: Boolean
})
</script>

<template>
  <router-link class="mv-button mv-text-button" :to="to" :inert="props.disabled" v-if="to">
    <slot></slot>
  </router-link>

  <button class="mv-button mv-text-button" type="button" :disabled="props.disabled" v-else>
    <slot></slot>
  </button>
</template>

<style lang="scss">
@import './commonStyles.scss';

.mv-text-button {
  padding: 0 0.75rem;
  --color: var(--primary-color);
  --background-color: none;

  &:hover:not(:disabled) {
    --background-color: var(--light-color);
  }

  &:disabled {
    opacity: 0.38;
    --color: var(--text-color);
  }
}
</style>