<script setup lang="ts">
import { onMounted, ref, watchEffect } from "vue";

import { create } from "@regions-of-indonesia/client";
import type { Region } from "@regions-of-indonesia/types";

import Label from "./components/Label.vue";
import Select from "./components/Select.vue";
import RegionSelectOptions from "./components/RegionSelectOptions.vue";

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
    <h1 class="mb-4 lg:mb-6 text-center text-lg md:text-xl 2xl:text-2xl font-mono font-bold">Regions of Indonesia</h1>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2 sm:gap-3 lg:gap-4">
      <div class="flex flex-col gap-1">
        <Label for="select-provinces">Provinces</Label>
        <Select id="select-provinces" v-model="selectedProvinceCode">
          <option value="" disabled>Select...</option>
          <RegionSelectOptions :regions="provinces" />
        </Select>
      </div>
      <div class="flex flex-col gap-1">
        <Label for="select-districts">Districts</Label>
        <Select id="select-districts" v-model="selectedDistrictCode">
          <option value="" disabled>Select...</option>
          <RegionSelectOptions :regions="districts" />
        </Select>
      </div>
      <div class="flex flex-col gap-1">
        <Label for="select-subdistricts">Subdistricts</Label>
        <Select id="select-subdistricts" v-model="selectedSubdistrictsCode">
          <option value="" disabled>Select...</option>
          <RegionSelectOptions :regions="subdistricts" />
        </Select>
      </div>
      <div class="flex flex-col gap-1">
        <Label for="select-villages">Villages</Label>
        <Select id="select-villages" v-model="selectedVillageCode">
          <option value="" disabled>Select...</option>
          <RegionSelectOptions :regions="villages" />
        </Select>
      </div>
    </div>
  </div>
</template>
