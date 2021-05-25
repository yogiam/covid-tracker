<template>
  <main v-if="!loading">
    <DataTitle :text="title" :fetchedOn="fetchedOn" />
    <DataBoxes :stats="stats" />
    <CountrySelect :countries="countries" />
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
