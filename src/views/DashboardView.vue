<script setup>
import { ref, onMounted } from "vue";
import CardInsight from '../components/CardInsight.vue';
import CardTable from "../components/CardTable.vue";

const users = ref([]);
const loading = ref(false);
const error = ref(null);

const fetchUserLists = () => {
  loading.value = true;
  try {
    return fetch('https://api.slingacademy.com/v1/sample-data/users?offset=0&limit=5', {
      method: 'get',
      headers: {
        'content-type': 'application/json'
      }
    }).then(async (res) => {
      const response = await res.json()

      users.value = response.users
    })
  } catch (err) {
    error.value = err;

    if (err.json) {
      return err.json.then(json => {
        error.value.message = json.message;
      });
    }
  } finally {
    loading.value = false;
  }
}

onMounted(() => {
  fetchUserLists();
});
</script>

<template>
  <div v-if="!!error">{{ error.message }}</div>
  <div class="flex flex-col gap-6">
    <div class="grid grid-flow-row grid-cols-2 lg:grid-cols-3 gap-4">
      <CardInsight status="Pending" :total="2480" :percentage="2.15" :loading="loading" />
      <CardInsight status="Paid" :total="82310" chart-type="down" :percentage="1.1" :loading="loading" />
      <CardInsight status="Rejected" :total="12155" :percentage="2.15" :loading="loading" />
    </div>
    <CardTable :loading="loading" :data="users" />
  </div>
</template>