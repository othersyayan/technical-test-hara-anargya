<script setup>
import { ref, computed } from "vue";

import { Icon } from "@iconify/vue";

import province from '../assets/js/province';
import city from '../assets/js/city'

const selectedProvince = ref(null);
const selectedCity = ref(null);
const showProvinceList = ref(false);
const showCities = ref(false);

const searchQuery = ref('');
const searchCity = ref('');

const sections = ref([]);

const sortDirectionProvince = ref('asc')
const sortDirectionCity = ref('asc')

const filteredAndSortedListCity = computed(() => {
  let filteredCities = city.filter(v => v.provinsi_id === selectedProvince.value.id)

  if (sortDirectionCity.value === 'asc') {
    return filteredCities.sort((a, b) => a.name.localeCompare(b.name));
  } else {
    return filteredCities.sort((a, b) => b.name.localeCompare(a.name));
  }
});

const filteredAndSortedListProvince = computed(() => {
  let filteredList = province.filter(item => item.name.toLowerCase().includes(searchQuery.value.toLowerCase()));

  if (sortDirectionProvince.value === 'asc') {
    return filteredList.sort((a, b) => a.name.localeCompare(b.name));
  } else {
    return filteredList.sort((a, b) => b.name.localeCompare(a.name));
  }
});

const scrollProvince = (id) => {
  sections.value[id].scrollIntoView({ behavior: 'smooth', block: 'center' });
}

</script>

<template>
  <div class="border rounded-lg bg-white overflow-y-auto">
    <div class="px-6 py-5 border-b">
      <p class="font-semibold">
        Detail Location
      </p>
    </div>
    <div class="grid grid-flow-row grid-cols-3 p-6 gap-6 md:gap-8">
      <div class="w-full">
        <label for="iprovince" class="text-sm font-semibold">
          Province
        </label>
        <div class="mt-2 flex border border-gray-200 rounded-lg">
          <input type="text" @input="(e) => {
            if (e.target.value !== '') {
              showProvinceList = true
            } else {
              showProvinceList = false
              selectedProvince = null
            }
          }" v-model="searchQuery" placeholder="Select Your Province"
            class="p-3 appearance-none outline-none w-full placeholder:text-[#AAAAAA] placeholder:text-sm text-sm text-[#727272] bg-gray-50"
            @keydown="showProvinceList = true">
          <div class="text-gray-300 w-10 border-l flex items-center justify-center border-gray-200">
            <button type="button" class="cursor-pointer font-bold outline-none focus:outline-none"
              @click="showProvinceList = !showProvinceList">
              <Icon :icon="showProvinceList ? 'solar:alt-arrow-down-outline' : 'solar:alt-arrow-up-outline'"
                class="stroke-1 stroke-[#424242] transition-all ease-in-out" width="18" height="18" />
            </button>
          </div>
        </div>

        <transition name="height-fade">
          <ul v-if="showProvinceList" class="mt-4 border rounded-lg max-h-64 overflow-y-auto"
            :class="filteredAndSortedListProvince.length ? 'h-64' : 'h-auto'" ref="wrapper-list">
            <li v-if="!filteredAndSortedListProvince.length" class="p-3 text-center">
              <p class="text-[#727272]/40 capitalize text-sm">Province not found.</p>
            </li>
            <div v-else class="relative">
              <li class="text-white text-sm cursor-pointer">
                <select v-model="sortDirectionProvince" @change="sortList"
                  class="p-3 appearance-none outline-none w-full placeholder:text-white placeholder:text-sm text-sm bg-blue-400 cursor-pointer relative">
                  <option value="asc">Sort Ascending</option>
                  <option value="desc">Sort Descending</option>
                </select>
              </li>
              <li v-for="item in filteredAndSortedListProvince" :key="item.id" :id="item.id" ref="sections"
                class="p-3 hover:bg-[#E3F2FD]/40 cursor-pointer text-[#424242] text-sm"
                :class="(selectedProvince && selectedProvince.id) === item.id ? 'bg-[#E3F2FD]' : ''" @click="(e) => {
                  const toLocation = e.target.id

                  if (!selectedProvince) {
                    selectedProvince = item
                    searchQuery = item.name

                    scrollProvince(toLocation)
                    showCities = true
                  } else if (selectedProvince && selectedProvince.id === item.id) {
                    selectedProvince = null
                    searchQuery = ''

                    selectedCity = null
                    searchCity = ''
                    showCities = false
                  } else {
                    selectedProvince = item
                    searchQuery = item.name

                    scrollProvince(toLocation)
                    showCities = true
                  }
                }">
                {{ item.name }}
              </li>
            </div>
          </ul>
        </transition>
      </div>

      <div class="w-full">
        <label for="ipcity" class="text-sm font-semibold">
          City
        </label>
        <div class="mt-2 flex border border-gray-200 rounded-lg">
          <input type="text" v-model="searchCity" placeholder="Select Your City" name="ipcity"
            class="p-3 appearance-none outline-none w-full placeholder:text-[#AAAAAA] placeholder:text-sm text-sm text-[#727272] transition-all ease-in"
            :class="selectedProvince ? 'bg-gray-50' : 'bg-gray-200/75 cursor-not-allowed'" :disabled="!selectedProvince"
            @keydown="showCities = true" @input="(e) => {
              if (e.target.value !== '') {
                showCities = true
              } else {
                showCities = false
                selectedCity = null
              }
            }" />

          <div class=" w-10 border-l flex items-center justify-center border-gray-200"
            :class="!selectedProvince ? 'bg-gray-100 text-gray-200' : 'text-gray-300'">
            <button type="button" class="font-bold outline-none focus:outline-none" @click="showCities = !showCities"
              :disabled="!selectedProvince" :class="!selectedProvince ? 'cursor-not-allowed' : 'cursor-pointer'">
              <Icon :icon="showCities ? 'solar:alt-arrow-down-outline' : 'solar:alt-arrow-up-outline'"
                class="stroke-1 stroke-[#424242] transition-all ease-in-out" width="18" height="18" />
            </button>
          </div>
        </div>

        <transition name="height-fade">
          <ul v-if="showCities" class="mt-4 border rounded-lg max-h-64 overflow-y-auto"
            :class="filteredAndSortedListCity.length ? 'h-64' : 'h-auto'" ref="wrapper-list">
            <li v-if="!filteredAndSortedListCity.length" class="p-3 text-center">
              <p class="text-[#727272]/40 capitalize text-sm">City not found.</p>
            </li>
            <div v-else class="relative">
              <li class="text-white text-sm cursor-pointer">
                <select v-model="sortDirectionCity" @change="sortList"
                  class="p-3 appearance-none outline-none w-full placeholder:text-white placeholder:text-sm text-sm bg-blue-400 cursor-pointer relative">
                  <option value="asc">Sort Ascending</option>
                  <option value="desc">Sort Descending</option>
                </select>
              </li>
              <li v-for="item in filteredAndSortedListCity" :key="item.id" :id="item.id"
                class="p-3 hover:bg-[#E3F2FD]/40 cursor-pointer text-[#424242] text-sm" ref="sectionsCity"
                :class="(selectedCity && selectedCity.id) === item.id ? 'bg-[#E3F2FD]' : ''" @click="() => {
                  if (!selectedCity) {
                    selectedCity = item
                    searchCity = item.name
                  } else if (selectedCity && selectedCity.id === item.id) {
                    selectedCity = null
                    searchCity = ''
                  } else {
                    selectedCity = item
                    searchCity = item.name
                  }
                }">
                {{ item.name }}
              </li>
            </div>
          </ul>
        </transition>
      </div>
    </div>
  </div>
</template>

<style scoped>
.height-fade-enter-active {
  transition: height 0.25s;
}

.height-fade-leave-active {
  transition: height 0.25s;
}

.height-fade-enter-from,
.height-fade-leave-to {
  height: 0;
}
</style>