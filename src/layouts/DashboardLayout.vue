<script setup>
import { RouterLink, RouterView } from "vue-router";
import { Icon } from '@iconify/vue';
</script>

<template>
  <div class="flex flex-row gap-0">
    <div class="w-1/6">
      <header class="w-full">
        <div class="wrapper bg-white border-r min-h-screen">
          <nav class="flex flex-col gap-2 p-4 md:p-6">
            <p class="font-extrabold text-[#152544] text-3xl mb-4">
              heylink.
            </p>
            <RouterLink to="/"
              class="flex flex-row gap-2 items-center text-base rounded-lg text-[#424242] p-2 hover:bg-[#E3F2FD]/40 transition ease-out"
              :active-class="'bg-[#E3F2FD]'" :exact-active-class="'bg-[#E3F2FD]'">
              <Icon icon="solar:home-smile-linear" width="20" height="20" />
              <span>Dashboard</span>
            </RouterLink>
            <RouterLink to="/overview"
              class="flex flex-row gap-2 items-center text-base rounded-lg text-[#424242] p-2 hover:bg-[#E3F2FD]/40 transition ease-out"
              :active-class="'bg-[#E3F2FD]'" :exact-active-class="'bg-[#E3F2FD]'">
              <Icon icon="carbon:analytics" width="20" height="20" />
              <span>Overview</span>
            </RouterLink>
          </nav>
        </div>
      </header>
    </div>
    <div class="w-5/6">
      <div class=" bg-white py-6 px-8 border-b font-semibold">
        Conversion
      </div>
      <main class="py-6 px-8">
        <RouterView />
      </main>
    </div>
  </div>
</template>
