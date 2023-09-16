<script setup lang="ts">
import { onMounted, ref, watchEffect } from "vue";

import { create } from "@regions-of-indonesia/client";
import type { Region } from "@regions-of-indonesia/types";

const client = create();

const provinces = ref<Region[]>([]);
const districts = ref<Region[]>([]);
const subdistricts = ref<Region[]>([]);
const villages = ref<Region[]>([]);

const selectedProvinceCode = ref<string>("");
const selectedDistrictCode = ref<string>("");
const selectedSubdistrictsCode = ref<string>("");
const selectedVillageCode = ref<string>("");

onMounted(async () => {
  try {
    provinces.value = await client.province.find();
  } catch (error) {
    provinces.value = [];
  }
});

watchEffect(async () => {
  selectedDistrictCode.value = "";

  try {
    districts.value = await client.district.find(selectedProvinceCode.value);
  } catch (error) {
    districts.value = [];
  }
});

watchEffect(async () => {
  selectedSubdistrictsCode.value = "";

  try {
    subdistricts.value = await client.subdistrict.find(selectedDistrictCode.value);
  } catch (error) {
    subdistricts.value = [];
  }
});

watchEffect(async () => {
  selectedVillageCode.value = "";

  try {
    villages.value = await client.village.find(selectedSubdistrictsCode.value);
  } catch (error) {
    villages.value = [];
  }
});
</script>

<template>
  <div class="container max-w-screen-lg mx-auto p-4 md:p-6 lg:p-8 xl:p-10">
    <h1 class="mb-4 lg:mb-6 text-center text-lg lg:text-xl font-mono">Regions of Indonesia</h1>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2 sm:gap-3 lg:gap-4">
      <div>
        <select class="select select-bordered select-xs w-full" v-model="selectedProvinceCode">
          <option value="" disabled>Select...</option>

          <option v-for="region in provinces" :key="region.code" :value="region.code">
            {{ region.name }}
          </option>
        </select>
      </div>
      <div>
        <select class="select select-bordered select-xs w-full" v-model="selectedDistrictCode">
          <option value="" disabled>Select...</option>

          <option v-for="region in districts" :key="region.code" :value="region.code">
            {{ region.name }}
          </option>
        </select>
      </div>
      <div>
        <select class="select select-bordered select-xs w-full" v-model="selectedSubdistrictsCode">
          <option value="" disabled>Select...</option>

          <option v-for="region in subdistricts" :key="region.code" :value="region.code">
            {{ region.name }}
          </option>
        </select>
      </div>
      <div>
        <select class="select select-bordered select-xs w-full" v-model="selectedVillageCode">
          <option value="" disabled>Select...</option>

          <option v-for="region in villages" :key="region.code" :value="region.code">
            {{ region.name }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>
