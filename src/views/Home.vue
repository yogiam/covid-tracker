<template>
  <main v-if="!loading">
    <DataTitle :text="title" :fetchedOn="fetchedOn" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <button @click="clearCountryData()"
      v-if="stats.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country
    </button>
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-grey-500 text-3xl mt-10 mb-6">
      Fetching data..
      <img :src="loadingImage" class="w-24 m-auto" />
    </div>
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      fetchedOn: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/loader.gif')
    }
  },
  methods: {
    async fetchCovidData() {
      const response = await fetch('https://api.covid19api.com/summary')
      const data = await response.json()
      return data
    },
    getCountryData(country) {
      this.stats = country
      this.title = country.Country 
    },
    async clearCountryData() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'Global'
      this.fetchedOn = data.Date
      this.stats = data.Global
      //this.countries = data.Countries
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    console.log(data)
    this.fetchedOn = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
}
</script>
