<script setup>
import { ref } from 'vue'
import ModalUser from './ModalUser.vue';

defineProps({
  loading: Boolean,
  data: Object
})

const selectedId = ref(0)
const isOpen = ref(false);

const handleSelectedId = (userId) => {
  if (selectedId.value !== userId) {
    selectedId.value = userId
  } else {
    selectedId.value = 0
  }
};

const handleOpenModal = (userId) => {
  selectedId.value = userId
  isOpen.value = true;
}

</script>

<template>
  <div class="border rounded-lg bg-white overflow-y-auto">
    <div class="px-6 py-5 border-b">
      <p class="font-semibold">
        User Lists
      </p>
    </div>
    <table class="w-full min-w-max">
      <thead class="bg-gray-50 shadow">
        <tr class="text-sm text-[#7C7C7C]">
          <th class="pl-4 py-4 text-left">ID</th>
          <th class="text-left pr-6">User</th>
          <th class="text-left pr-6">Date Of Birth</th>
          <th class="text-left pr-6">Email</th>
          <th class="text-left pr-6">Job</th>
          <th class="text-left pr-6">Country</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, i) in data" :key="i" class="border-b hover:bg-gray-50"
          :class="selectedId === user.id ? 'bg-gray-100' : ''">
          <td class="text-sm text-[#686868] p-5">{{ user.id }}</td>
          <td class="flex flex-row gap-2 items-center text-sm text-[#080808] py-5">
            <img :src="user.profile_picture" :alt="user.first_name" width="20" height="20" class="rounded-full">
            <span class="text-[#080808]">{{ `${(user.first_name ?? '')} ${(user.last_name ?? '')}` }}</span>
          </td>
          <td class="text-sm text-[#686868] text-left">
            {{
              `${new Date(user.date_of_birth).getDate()} / 0${new Date(user.date_of_birth).getMonth()} / ${new
                Date(user.date_of_birth).getFullYear()}`
            }}
          </td>
          <td class="text-sm text-[#080808] text-left">
            {{ user.email }}
          </td>
          <td class="text-sm text-[#686868] text-left">
            {{ user.job }}
          </td>
          <td class="text-sm text-[#080808] text-left">
            {{ user.country }}
          </td>
          <td class="text-sm pl-4">
            <button class="bg-white border rounded-lg px-3 py-2 hover:border-[#080808]/50 text-xs font-semibold mr-2"
              :class="selectedId === user.id ? 'border-[#080808] bg-white/5' : 'bg-white'"
              @click="handleSelectedId(user.id)">
              Select
            </button>
            <button class="border rounded-lg px-3 py-2 hover:border-[#080808]/50 text-xs font-semibold"
              @click="handleOpenModal(user.id)"
              :class="selectedId === user.id && isOpen ? 'border-[#080808] bg-white/5' : 'bg-white'">
              View Details
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <ModalUser :isOpen="isOpen" @update:isOpen="isOpen = $event" @update:selectedId="selectedId = $event"
    :userId="selectedId" />
</template>