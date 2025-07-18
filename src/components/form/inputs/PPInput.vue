<template>
  <div class="input-wrapper">
    <div :class="['input-container', { 'has-error': hasError }]">
      <label :for="id" :class="['floating-label', { active: isFocused || value }]">
        {{ label }}
      </label>

      <input
        :id="id"
        class="input-element"
        :name="name"
        :type="type"
        :value="value"
        @input="onInput"
        @focus="isFocused = true"
        @blur="(isFocused = false) || emit('blur')"
      />
    </div>

    <div class="error-message">
      <span v-if="hasError">{{ error }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const props = defineProps<{
  type: 'text' | 'number' | 'email' | 'password' | 'tel',
  name: string,
  label: string;
  error?: string;
}>();

const emit = defineEmits<{
  (event: 'blur'): void;
}>();

const id = `input-${Math.random().toString(36).substring(2, 10)}`;
const value = ref<string>('');
const isFocused = ref(false);
const hasError = computed(() => !!props.error);

const onInput = (event: Event) => {
  const target = event.target as HTMLInputElement;

  value.value = target.value;
};
</script>

<style scoped>
.input-wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.input-container {
  width: 100%;
  position: relative;
  border: 1px solid var(--pp-secondary-light-color);
  background-color: transparent;
  border-radius: var(--pp-border-radius-md);
  transition: border-color 0.2s, background-color 0.2s;
}

.input-container.has-error {
  border-color: var(--pp-error-color);
  background-color: var(--pp-error-light-color);
}

.input-container.has-error .floating-label {
  color: var(--pp-error-color);
}

.input-element {
  width: 100%;
  border: none;
  outline: none;
  padding: var(--pp-gap-md) var(--pp-gap-md) var(--pp-gap-sm) var(--pp-gap-md);
  background: transparent;
  font-size: var(--pp-font-size-md);
  color: var(--pp-text-color);
}

.floating-label {
  position: absolute;
  top: calc(var(--pp-gap-md) - var(--pp-gap-sm) / 2);
  left: var(--pp-gap-md);
  font-size: var(--pp-font-size-md);
  color: var(--pp-secondary-color);
  pointer-events: none;
  transition: 0.2s ease all;
}

.floating-label.active {
  top: calc(var(--pp-gap-xs) / 5);
  font-size: var(--pp-font-size-xs);
}

.error-message {
  margin-top: var(--pp-gap-xs);
  padding-left: var(--pp-gap-xs);
  font-size: var(--pp-font-size-xs);
  color: var(--pp-error-color);
}
</style>
