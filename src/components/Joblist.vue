<script lang="ts">
import { defineComponent, PropType, ref } from 'vue';
import Job from '../types/job';
import OrderTerm from '../types/orderterm';

export default defineComponent({
  name: 'JobList',
  props: {
    jobs: {
      type: Array as PropType<Job[]>,
      required: true,
    },
    order: {
      type: String as PropType<OrderTerm>,
      required: true,
    },
  },
});
</script>

<template>
  <main>
    <div class="">
      <p class="text-lg font-bold mt-6">Ordered by {{ order }}</p>
      <ul class="mt-4">
        <li v-for="job in jobs" :key="job.id" class=" mb-4 p-4 backdrop-blur-sm bg-slate-100 rounded-md">
          <p class="font-bold text-neutral-700">{{ job.title }}</p>
          <p class="text-neutral-700">{{ job.location }}</p>
          <p class="text-neutral-700">{{ job.salary }}</p>
        </li>
      </ul>
    </div>
  </main>
</template>
