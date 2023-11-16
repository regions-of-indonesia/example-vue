<script lang="ts" setup>
import clsx from "clsx";

defineOptions({
  inheritAttrs: false,
});
</script>

<template>
  <label v-bind="{ ...$attrs, class: undefined }" :class="clsx('text-neutral-11 text-base font-medium', $attrs.class as any)">
    <slot></slot>
  </label>
</template>
