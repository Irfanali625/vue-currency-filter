<script>
import axios from 'axios';

export default {
  data() {
    return {
      data: [],
      selectedCountry: '',
      selectedStatus: '',
      selectedRange: null,
    };
  },
  mounted() {
    this.fetchData();
  },
  computed: {
    filteredData() {
      let filteredCards = this.data;

      if (this.selectedCountry !== '') {
        filteredCards = filteredCards.filter(item => item.country_name.toLowerCase() === this.selectedCountry.toLowerCase());
      }

      if (this.selectedStatus !== '') {
        filteredCards = filteredCards.filter(item => item.status.toLowerCase() === this.selectedStatus.toLowerCase());
      }

      if (this.selectedRange !== null) {
        filteredCards = filteredCards.filter(item => item.value === Number(this.selectedRange));
      }

      return filteredCards;
    },
  },
  methods: {
    fetchData() {
      axios.get('/country.json')
        .then(response => {
          this.data = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    clearFilters() {
      this.selectedCountry = '';
      this.selectedStatus = '';
      this.selectedRange = null;
    },
  },
};

</script>

<template>
  <div class="container mx-auto m-4">
 
    <!-- {{ data }} -->
    <h1 class="text-3xl">Search Old Currency</h1>
    <div class="flex justify-center m-2 bg-gray-100 p-2 rounded">
      <div class="mx-6">
        <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Country</label>
      <select v-model="selectedCountry" @change="filterCards" id="countries" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
        <option selected value="">Choose a Country</option>
        <option value="Pakistan">Pakistan</option>
        <option value="China">China</option>
        <option value="India">India</option>
        <option value="Usa">Usa</option>
        <option value="Saudia">Saudia</option>
        <option value="Egypt">Egypt</option>
      </select>
      </div>
      <!-- <div class="mx-6">
        <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Year</label>
        <input type="date" name="" id="" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
      </div> -->
       <div class="mx-6">
          <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Country</label>
        <select v-model="selectedStatus" @change="filterCards" id="countries" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
          <option selected value="">Status</option>
          <option  value="Valid">Valid</option>
          <option value="Demonotized">Demonotized</option>
        </select>
        </div>
        <div class="mx-6">
          <label for="range" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Notes Range</label>
          <input type="range" v-model="selectedRange" @input="filterCards" min="0" max="500" step="1" oninput="this.nextElementSibling.value = this.value">
          <output>500</output>
        </div>
        <button @click="clearFilters" class="bg-blue-700 text-white m-5 p-2 rounded">Clear All</button>
        </div>

      <div class="flex justify-center mx-0 md:mx-24">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <div v-for="item in  filteredData" :key="item.id" class="max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">
       <a href="#">
           <img class="rounded-t-lg" :src="item.front_pic" alt="" />
       </a>
       <div class="p-5">
        <div class="flex justify-between">
          <a href="#">
              <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{{item.country_name }}</h5>
          </a>
            <a href="#" class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                Bid
            </a>
        </div>
           <h3>{{ item.year }}</h3>
           <p>{{ item.status }}</p>
           <p class="hidden">{{ item.value }}</p>
           <p class="hidden">{{ item.range }}</p>
           <p class="mb-3 font-normal text-gray-700 dark:text-gray-400">{{item.history}}</p>
       </div>
   </div>
   </div>
 </div>
    </div>

</template>

<style scoped>

</style>
