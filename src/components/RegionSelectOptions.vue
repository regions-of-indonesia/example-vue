<script lang="ts" setup>
import type { Region } from "@regions-of-indonesia/types";

defineProps<{
  regions: Region[];
}>();
</script>

<template>
  <option v-for="region in regions" :key="region.code" :value="region.code">
    {{ region.name }}
  </option>
</template>
