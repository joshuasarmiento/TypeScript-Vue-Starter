<template>
  <main class="flex flex-col items-center justify-center">

    <div class="flex p-4 space-x-6">
      <a href="https://vitejs.dev" target="_blank">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
      </a>
      <a href="https://vuejs.org/" target="_blank">
        <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
      </a>
    </div>
    
    <form @submit.prevent="createJob">
      <div class="flex space-x-4 mb-4">
        <input v-model="newJob.title" required type="text" placeholder="Title" class="border border-gray-300 rounded-md p-2" />
        <input v-model="newJob.location" required type="text" placeholder="Location" class="border border-gray-300 rounded-md p-2" />
        <input v-model.number="newJob.salary" required type="number" placeholder="Salary" class="border border-gray-300 rounded-md p-2" />
      </div>
      <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Create Job</button>
    </form>

    <div class="flex space-x-4 mt-6">
      <button class="" @click="handleClick('title')">Sort By Title</button>
      <button class="" @click="handleClick('location')">Sort By Location</button>
      <button class="" @click="handleClick('salary')">Sort By Salary</button>
    </div>
    <br>
    <JobList :jobs="sortedJobs" :order="order"/>
  </main>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
import Job from './types/job';
import jobData from './data/job.json';
import JobList from './components/Joblist.vue';
import OrderTerm from './types/orderterm';

export default defineComponent({
  name: 'App',
  components: {
    JobList
  },
  setup() {
    const jobs = ref<Job[]>(jobData);

    const newJob = ref<Job>({
      id: 0,
      title: '',
      location: '',
      salary: 0
    });

    const sortByTitle = () => {
      jobs.value = [...jobs.value].sort((a, b) => a.title.localeCompare(b.title));
    };

    const sortByLocation = () => {
      jobs.value = [...jobs.value].sort((a, b) => a.location.localeCompare(b.location));
    };

    const sortBySalary = () => {
      jobs.value = [...jobs.value].sort((a, b) => a.salary - b.salary);
    };

    const order = ref<OrderTerm>('title');

    const handleClick = (term: OrderTerm) => {
      if (term === 'title') {
        sortByTitle();
      } else if (term === 'location') {
        sortByLocation();
      } else if (term === 'salary') {
        sortBySalary();
      }
      order.value = term;
    };

    const sortedJobs = computed(() => {
      if (order.value === 'title') {
        return [...jobs.value].sort((a, b) => a.title.localeCompare(b.title));
      } else if (order.value === 'location') {
        return [...jobs.value].sort((a, b) => a.location.localeCompare(b.location));
      } else if (order.value === 'salary') {
        return [...jobs.value].sort((a, b) => a.salary - b.salary);
      }
      return jobs.value;
    });

    const createJob = () => {
      // Generate a unique ID for the new job
      const newJobId = jobs.value.length + 1;

      // Create a new job object with the input values
      const job: Job = {
        id: newJobId,
        title: newJob.value.title,
        location: newJob.value.location,
        salary: newJob.value.salary
      };

      // Push the new job to the jobs array
      jobs.value.push(job);

      
      // Reset the input values
      newJob.value = {
        id: 0,
        title: '',
        location: '',
        salary: 0
      };
    };


    return {
      jobs,
      order,
      handleClick,
      sortedJobs,
      newJob,
      createJob,
    };
  }
});
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
