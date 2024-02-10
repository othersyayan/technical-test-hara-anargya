<script setup>
import { ref, watchEffect } from "vue";

import { Icon } from '@iconify/vue';

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  userId: {
    type: Number,
    default: 0
  }
})

const user = ref(null);
const loading = ref(true);
const error = ref(null);
const emit = defineEmits();

const close = () => {
  emit('update:isOpen', false);
  emit('update:selectedId', '');
}

watchEffect(async () => {
  if (props.isOpen && props.userId > 0) {
    loading.value = true
    try {
      await fetch(
        `https://api.slingacademy.com/v1/sample-data/users/${props.userId}`
      ).then(async (res) => {
        const response = await res.json()

        user.value = response.user
      })
    } catch (err) {
      error.value = err

      if (err.json) {
        return err.json.then(json => {
          error.value.message = json.message;
        });
      }
    } finally {
      loading.value = false;
    }
  }
})
</script>

<template>
  <transition name="slide-fade">
    <div v-if="props.isOpen" class="fixed inset-0 z-50 overflow-y-auto flex items-center justify-center">
      <div class="fixed inset-0 bg-black opacity-50"></div>

      <div class="relative bg-white rounded-lg p-8 max-w-max">
        <button @click="close" class="absolute top-2 right-2 p-2">
          <svg class="h-6 w-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
          </svg>
        </button>

        <div class="w-full flex flex-row items-center justify-center" v-if="loading && !user">
          <svg class="animate-spin -ml-1 mr-3 h-8 w-8 text-gray-800" xmlns="http://www.w3.org/2000/svg" fill="none"
            viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
            </path>
          </svg>
        </div>

        <div v-else>
          <div class="flex flex-row gap-4 md:gap-6 items-center">
            <img :src="user.profile_picture" :alt="user.first_name + user.id + user.last_name" class="w-20 rounded-full">
            <div>
              <p class="text-xl font-bold mb-1">
                <span>
                  {{ user.first_name ?? '' }} {{ user.last_name ?? '' }}
                </span>
              </p>
              <p class="text-sm mb-0.5 flex items-center gap-1.5">
                <span class="text-[#080808]">
                  <Icon icon="solar:phone-outline" width="16" height="16" />
                </span>

                <span class="text-[#686868]">
                  {{ user.phone ?? '-' }}
                </span>
              </p>
              <p class="text-sm mb-0.5 flex items-center gap-1.5">
                <span class="text-[#080808]">
                  <Icon icon="solar:letter-outline" width="16" height="16" />
                </span>

                <span class="text-[#686868]">
                  {{ user.email ?? '-' }}
                </span>
              </p>
            </div>
          </div>
          <div class="grid grid-rows-4 grid-flow-col grid-cols-1 my-6">
            <div class="inline-flex gap-2 mb-2">
              <p class="text-sm font-semibold">
                Gender:
              </p>
              <p class="text-sm text-[#686868] capitalize">
                {{ user.gender ?? '-' }}
              </p>
            </div>
            <div class="inline-flex gap-2 mb-2">
              <p class="text-sm font-semibold">
                Date of Birth:
              </p>
              <p class="text-sm text-[#686868] capitalize">
                {{
                  `${new Date(user.date_of_birth).getDate()} / 0${new Date(user.date_of_birth).getMonth()} / ${new
                    Date(user.date_of_birth).getFullYear()}`
                }}
              </p>
            </div>
            <div class="inline-flex gap-2 mb-2">
              <p class="text-sm font-semibold">
                Job:
              </p>
              <p class="text-sm text-[#686868] capitalize">
                {{ user.job }}
              </p>
            </div>

            <div class="inline-flex gap-2 mb-2">
              <p class="text-sm font-semibold">
                Address:
              </p>
              <p class="text-sm text-[#686868] capitalize">
                {{ user.street ?? '' }}, {{ user.city ?? '' }}, {{ user.state ?? '' }}, {{ user.country ?? '' }}, {{
                  user.zipcode ?? '' }}
              </p>
            </div>
          </div>
          <iframe :src="`https://www.google.com/maps?q=${user.latitude}, ${user.longitude}&z=15&output=embed`" width="600"
            height="300" style="border:0;" allowfullscreen="" loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"></iframe>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.slide-fade-enter-active {
  transition: all 0.25s ease-in;
}

.slide-fade-leave-active {
  transition: all 0.25s ease-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  opacity: 0;
}
</style>