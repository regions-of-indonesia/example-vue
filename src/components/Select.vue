<script lang="ts" setup>
import { computed } from "vue";

import clsx from "clsx";

defineOptions({
  inheritAttrs: false,
});

const props = defineProps(["modelValue"]);
const emits = defineEmits(["update:modelValue"]);

const value = computed({
  get() {
    return props.modelValue;
  },
  set(value) {
    emits("update:modelValue", value);
  },
});
</script>

<template>
  <select
    v-bind="{ ...$attrs, class: undefined }"
    v-model="value"
    :class="clsx('block w-full bg-neutral-1 text-neutral-12 focus:border-primary-9 rounded-xl focus:ring-primary-9', $attrs.class as any)"
  >
    <slot></slot>
  </select>
</template>
