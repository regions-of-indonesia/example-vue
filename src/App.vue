<script setup lang="ts">
import { onMounted, ref, watchEffect } from "vue";

import { create, cache } from "@regions-of-indonesia/client";
import { isRegionCode } from "@regions-of-indonesia/utils";
import type { Region } from "@regions-of-indonesia/types";

import Label from "./components/Label.vue";
import Select from "./components/Select.vue";
import RegionSelectOptions from "./components/RegionSelectOptions.vue";

const client = create({
  middlewares: [cache()],
});

const parseRegionCode = (value: unknown) => {
  if (value && isRegionCode(value)) return value;
  throw new Error("Invalid region code");
};

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
    console.error(error);
    provinces.value = [];
  }
});

watchEffect(async () => {
  selectedDistrictCode.value = "";

  try {
    districts.value = await client.district.find(parseRegionCode(selectedProvinceCode.value));
  } catch (error) {
    console.error(error);
    districts.value = [];
  }
});

watchEffect(async () => {
  selectedSubdistrictsCode.value = "";

  try {
    subdistricts.value = await client.subdistrict.find(parseRegionCode(selectedDistrictCode.value));
  } catch (error) {
    console.error(error);
    subdistricts.value = [];
  }
});

watchEffect(async () => {
  selectedVillageCode.value = "";

  try {
    villages.value = await client.village.find(parseRegionCode(selectedSubdistrictsCode.value));
  } catch (error) {
    console.error(error);
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
